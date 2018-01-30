# Drivers

- [Introduction](#introduction)
- [Defining a driver](#defining-a-driver)
    - [Setup](#setup)
    - [Receiving a message](#receiving-a-message)
    - [Handling users](#handling-users)
    - [Answering conversations](#answering-conversations)
    - [Sending a message](#sending-a-message)
    - [Typing indicators](#typing-indicators)
    - [Activation](#activation)
- [Message templates](#message-templates)

<a id="introduction"></a>
## Introduction

BotMan ships with support for a number of different messaging channels. Each channel is powered by it's own driver which can be installed either manually or via [BotMan Studio](/__version__/botman-studio).

Should you need to utilise a messaging channel not supported by BotMan, it is possible to define your own. Below is a step-by-step guide of how to do so.

<a id="defining-a-driver"></a>
### Defining a driver

A driver is a class which extends `BotMan\BotMan\Drivers\HttpDriver`.

This parent class defines a blueprint by which the driver must adhere to. Succesfully implementing the methods set out in this class, results in a fully working driver. 

```
<?php

namespace App\Drivers;

use BotMan\BotMan\Drivers\HttpDriver;

class MyDriver extends HttpDriver
{
}
```

<a id="setup"></a>
### Setup

When each driver is instantiated by BotMan, the `buildPayload()` method is called. This method is used to carry out the initial setup. In the case of Facebook, it looks like this.

```
/**
* @param Request $request
*/
public function buildPayload(Request $request)
{
    $this->payload = new ParameterBag((array) json_decode($request->getContent(), true));
    $this->event = Collection::make((array) $this->payload->get('entry')[0]);
    $this->signature = $request->headers->get('X_HUB_SIGNATURE', '');
    $this->content = $request->getContent();
    $this->config = Collection::make($this->config->get('facebook', []));
}
```

BotMan needs to be notified whether or not the driver can handle the incoming request. This is handled by the `matchesRequest()` method.

This method interogates the incoming request to determine whether it contains anything that the driver is expecting and simply returns a boolean.

In the case of Slack, we know it's a Slack message if the incoming event contains a user, `team_domain` or `bot_id`.

```
public function matchesRequest()
{
    return ! is_null($this->event->get('user')) || ! is_null($this->event->get('team_domain')) || ! is_null($this->event->get('bot_id'));
}
```

<a id="receiving-a-message"></a>
### Receiving a message

Once it has been determined that the driver should handle the request, BotMan needs to have access to the messages that have been received.

The parent `HttpDriver` class contains a `messages` property that needs to be populated. In order to do this, the `getMessages` method needs to be defined.

BotMan is expecting the `messages` property to contain an array of `BotMan\BotMan\Messages\Incoming\IncomingMessage` objects.

It's important to note that even if the request only contains a single message, the `messages` property should be an array.

```
/**
* Retrieve the chat message.
*
* @return array
*/
public function getMessages()
{
    if (empty($this->messages)) {
        $message = $this->event->get('message');
        $userId = $this->event->get('userId');
        $this->messages = [new IncomingMessage($message, $userId, $userId, $this->payload)];
    }
    return $this->messages;
}
```

`IncomingMessage` expects a message string, sender ID, recipient ID and optional payload when instantiating the class. 

<a id="handling-users"></a>
### Handling users

BotMan ships with a class for managing users: `BotMan\BotMan\Users\User`

To set this up simply return a new `User` object from the `getUser` method of the driver.

```
**
* Retrieve User information.
* @param IncomingMessage $matchingMessage
* @return UserInterface
*/
public function getUser(IncomingMessage $matchingMessage)
{
    return new User($matchingMessage->getSender());
}
```

<a id="answering-conversations"></a>
### Answering conversations

For your bot to respond to conversations, the `getConversationAnswer` method needs to be defined.

From here, simply return an instance of `BotMan\BotMan\Messages\Incoming\Answer`

```
/**
* @param IncomingMessage $message
* @return \BotMan\BotMan\Messages\Incoming\Answer
*/
public function getConversationAnswer(IncomingMessage $message)
{
    return Answer::create($message->getText())->setMessage($message);
}
```

If your driver supports interactive replies such as postbacks from buttons, pass a boolean value to the `setInteractiveReply` method of the `Answer object`. This will let BotMan know whether or not it is a standard or interactive message.

Take a look at the other methods of this class as there may be other features useful for your driver.

<a id="sending-a-message"></a>
### Sending a message

In order for your bot to send a message, there are two methods that need defining.

The `buildServicePayload` method receives the message instance to be sent by BotMan. The purpose of this method is to take the message and transform it into the payload required by your driver.

```
/**
* @param string|Question|OutgoingMessage $message
* @param IncomingMessage $matchingMessage
* @param array $additionalParameters
* @return Response
*/
public function buildServicePayload($message, $matchingMessage, $additionalParameters = [])
{
    if (! $message instanceof WebAccess && ! $message instanceof OutgoingMessage) {
        $this->errorMessage = 'Unsupported message type.';
        $this->replyStatusCode = 500;
    }
    return [
        'message' => $message,
        'additionalParameters' => $additionalParameters,
    ];
}
```

Closely coupled with this method is `sendPayload`. The responsibility of this method is to take the payload created above and send it.

Your driver has access to a HTTP client which can assist with this.

The example below posts the payload to a given endpoint and handles the addtion of any required headers.

```
/**
* @param mixed $payload
* @return Response
*/
public function sendPayload($payload)
{
    $response = $this->http->post($this->endpoint . 'send', [], $payload, [
        "Authorization: Bearer {$this->config->get('token')}",
        'Content-Type: application/json',
        'Accept: application/json',
    ], true);

    return $response;
}
```

<a id="activation"></a>
### Activation

There are two steps required to activate a driver.

Firstly, define the `isConfigured` method of the driver. This should return a boolean as to whether or not the driver is ready for use. Typically, it's a good idea to check any required configuration has been set for the driver in this method.

```
/**
* @return bool
*/
public function isConfigured()
{
    return !empty($this->config->get('token'));
}
```

Now, BotMan needs to be informed that the driver exists.

If you are using BotMan studio for Laravel, this is easy. Simply open `App\Providers\BotMan\DriverServiceProvider` and add your driver to the `$drivers` array.

If you are using the package in a different environment, use the `DriverManager` to load your driver as follows:

```
use BotMan\BotMan\BotManFactory;
use BotMan\BotMan\Drivers\DriverManager;

DriverManager::loadDriver(FacebookDriver::class);
BotManFactory::create(config('botman'));
``` 

<a id="message-templates"></a>
## Message templates

If your driver offers message templates not supported by BotMan out of the box, you may define your own.

Simply define a class for the template which will be used to build up all of the available elements that comprise it.

This object is passed to the `buildServicePayload` method of your driver. As such, it's useful to define a method which will transform the object into a format that can be sent to your endpoint.

Below is an example of a template which contains text and buttons:

```
class ExampleTemplate implements \JsonSerializable
{
    /** @var string */
    protected $text;

    /** @var array */
    protected $buttons = [];

    /**
     * @param $text
     * @return static
     */
    public static function create($text)
    {
        return new static($text);
    }

    public function __construct($text)
    {
        $this->text = $text;
    }

    /**
     * @param $button
     * @return $this
     */
    public function addButton(ButtonTemplate $button)
    {
        $this->buttons[] = $button->toArray();
        return $this;
    }

    /**
     * @param array $buttons
     * @return $this
     */
    public function addButtons(array $buttons)
    {
        foreach ($buttons as $button) {
            if ($button instanceof ButtonTemplate) {
                $this->buttons[] = $button->toArray();
            }
        }

        return $this;
    }

    /**
     * @return array
     */
    public function toArray()
    {
        return [
            'type' => 'action',
            'message' => [[
                'text' => $this->text,
                'buttons' => $this->buttons,
            ]],
        ];
    }

    /**
     * @return array
     */
    public function jsonSerialize()
    {
        return $this->toArray();
    }
}
``` 