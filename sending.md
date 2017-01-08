# Sending Messages

- [Introduction](#introduction)
- [Single Message Replies](#single-message-replies)
- [Multi Message Replies](#multi-message-replies)

## Introduction

Bots have to send messages to deliver information and present an interface for their
functionality.  BotMan bots can send messages in several different ways, depending
on the type and number of messages that will be sent.

Single message replies to incoming commands can be sent using the `$bot->reply()` function.

Multi-message replies, particularly those that present questions for the end user to respond to,
can be sent using the `$bot->startConversation()` function and the related conversation sub-functions. 

Bots can originate messages - that is, send a message based on some internal logic or external stimulus - using `$bot->say()` method.

<a id="single-message-replies"></a>

## Single Message Replies

Once a bot has received a message using `hears()`, a response
can be sent using `$bot->reply()`.

Messages sent using `$bot->reply()` are sent immediately. If multiple messages are sent via
`$bot->reply()` in a single event handler, they will arrive in the  client very quickly
and may be difficult for the user to process. We recommend using `$bot->startConversation()`
if more than one message needs to be sent.

You may pass either a string, a `Message` object or a `Question` object to the function.

As a second parameter, you may also send any additional fields to pass along the configured driver.

#### $bot->reply()

| Argument | Description
|--- |---
| reply | _String_ or _Message_ or _Question_ Outgoing response
| additionalParameters | _Optional_ Array containing additional parameters

Simple reply example:

```php
$botman->hears('keyword', function (BotMan $bot) {
    // do something to respond to message
    // ...

    $bot->reply("Tell me more!");
});
```

You can also compose your message using the `Mpociot\BotMan\Messages\Message` class to have a unified API to add images 
to your chat messages. 

```php
use Mpociot\BotMan\Messages\Message;

$botman->hears('keyword', function (BotMan $bot) {
    // Build message object
    $message = Message::create('This is my text')
                ->image('http://www.some-url.com/image.jpg');
    // Reply message object
    $bot->reply($message);
});
```


Slack-specific fields and attachments:

```php
$botman->hears('keyword', function (BotMan $bot) {
    // do something...

    // then respond with a message object
    $bot->reply("A more complex response",[
        'username' => "ReplyBot",
        'icon_emoji' => ":dash:",
    ]);
})
```

<a id="multi-message-replies"></a>
## Multi Message Replies

For more complex commands, multiple messages may be necessary to send a response,
particularly if the bot needs to collect additional information from the user.

BotMan provides a `Conversation` object that is used to string together several
messages, including questions for the user, into a cohesive unit. BotMan conversations
provide useful methods that enable developers to craft complex conversational
user interfaces that may span several minutes of dialog with a user, without having to manage
the complexity of connecting multiple incoming and outgoing messages across
multiple API calls into a single function.