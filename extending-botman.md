# Extending BotMan

- [Custom Driver](#custom-driver)
- [Custom Middleware](#custom-middleware)

<a id="custom-driver"></a>
## Custom Driver

BotMan already has a a great number of available messaging services out of the box, but from time to time you might find yourself in 
a situation where you want to either use a completely new messaging service, like your own backend API, or maybe you just want to enhance
a given driver.

When you want to create your own custom driver from scratch, all you need to do is extend the abstract `Driver` class that BotMan provides.
It contains all methods that are necessary to read and send messages using your new driver.

Let's take a look at a very simple implementation and what the different abstract and interface methods mean for you:

```php

class CustomDriver extends Driver
{
    protected $myData = [];

    /**
     * @param Request $request
     * @return void
     */
    public function buildPayload(Request $request)
    {
        // This method receives the incoming HTTP Request and allows you
        // to read the driver relevant information from it.
        $this->myData = $request->request->all();
    }

    /**
     * Determine if the request is for this driver.
     *
     * @return bool
     */
    public function matchesRequest()
    {
        // This method detects if the incoming HTTP request should be handled with this driver class.
        return isset($this->myData['driver_specific_request']);
    }

    /**
     * Retrieve the chat message(s).
     *
     * @return array
     */
    public function getMessages()
    {
        // Return the message(s) that are inside the incoming request.
        return [new Message($this->myData['text'], $this->myData['sender_id'], $this->myData['recipient_id'])];
    }

    /**
     * @return bool
     */
    public function isBot()
    {
        // If the custom messaging service also sends HTTP requests for the bot 
        // replies, you need to handle this here.
        return $this->myData['is_bot'];
    }

    /**
     * @return bool
     */
    public function isConfigured()
    {
        return $this->config->has('foo');
    }

    /**
     * Retrieve User information.
     * @param Message $matchingMessage
     * @return UserInterface
     */
    public function getUser(Message $matchingMessage)
    {
        // Return a user object with as many information as you want.
        return new User($matchingMessage->getChannel());
    }

    /**
     * @param Message $matchingMessage
     *
     * @return Answer
     */
    public function getConversationAnswer(Message $message)
    {
        // Return the given answer, when inside a conversation.
        return Answer::create($message->getMessage());
    }

    /**
     * @param string|Question $message
     * @param Message $matchingMessage
     * @param array $additionalParameters
     * @return $this
     */
    public function reply($message, $matchingMessage, $additionalParameters = [])
    {
    	// Send a reply to the messaging service.
    	// Replies can either be strings, Question objects or IncomingMessage objects.
        if ($message instanceof Question) {
            $additionalParameters['text'] = $message->getText();
        } elseif ($message instanceof IncomingMessage) {
            $additionalParameters['text'] = $message->getMessage();
        } else {
            $additionalParameters['text'] = $message;
        }
        $this->http->post('http://foo.bar', [], $additionalParameters);
    }

    /**
     * Return the driver name.
     *
     * @return string
     */
    public function getName()
    {
        return 'MyCustomDriver';
    }
}
```

To make use of your new driver, you first have to tell BotMan's DriverManager about it:

```php
// Load the driver
DriverManager::loadDriver(CustomDriver::class);
```

BotMan will now try to match all incoming requests with your new driver as well.

You can of couse use the custom driver, like the built-in drivers and restrict certain messages to it:

```php
$botman->hears('keyword', function(){
	
})->driver(CustomDriver::class);
```

<a id="custom-middleware"></a>
## Custom Middleware

Middleware is a powerful concept for BotMan. The usage of middleware allows BotMan to listen for different information, or enrich the message with
new information. Similar to the custom driver, your custom middleware class needs to implement a `MiddlewareInterface`.

Let's take a look at an easy Authorization middleware.

```php
class Authorization implements MiddlewareInterface
{

    protected $allowedUsers = [1, 2, 3];

    /**
     * @param Message $message
     * @param string $test
     * @param bool $regexMatched Indicator if the regular expression was matched too
     * @return bool
     */
    public function isMessageMatching(Message $message, $test, $regexMatched)
    {
        return in_array($message->getChannel(), $this->allowedUsers);
    }
}
```

This middleware checks if the user sending the message is inside a fixed list of user ids. Regardless if the regular expression of your BotMan `hears` commands matches, this middleware will restrict access to only three users.

If you want to add some more information to your message, you can do this by using the `handle` method of the middleware:



```php
class EnrichMessage implements MiddlewareInterface
{
    /**
     * Handle / modify the message.
     *
     * @param Message $message
     */
    public function handle(Message &$message)
    {
		$message->addExtras('customInformation', [
			'foo' => 'bar',
		]);
    }
}
```

Then again, use the custom middleware just as the built-in ones:

```php
$botman->hears('keyword', function(){
	
})->middleware(new Authorization());
```
