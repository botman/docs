# BotMan Release Notes
<br><br>
## 2.0
BotMan 2.0 is a new major release and continues the ideas of BotMan 1.5 by adding a new middleware system, support for messaging service events, painless driver installation and listing, a chatbot testing framework on top of PHPUnit, custom exception handling, automatic messaging service verification, extended messaging service user information, support for additional drivers such as Kik, Discord or Cisco Spark, sending file attachments, and more.

## Driver Events
BotMan 2.0 gives you the ability to listen not only for incoming messages of your users, but also for service events. 
Need some inspiration?
You can greet new users that join a Slack channel, or perform a specific task, whenever your Facebook messages were read.

BotMan 2.0 makes this really easy. Just call the `on` method on the BotMan instance:

```php
/**
 * Greet a new user when he joins a Slack channel.
 */
$botman->on('team_join', function($payload, $botman) {
	$botman->reply('Hello!');
});

/**
 * Perform an action when a message was read on Facebook.
 */
$botman->on('messaging_reads', function($payload, $botman) {
	// a message was read.
});
```

For more information on driver events, check out the <a href="/__version__/events">driver event documentation</a>.

## Testing
With BotMan 2.0 you have the ability to write fluent, expressive unit tests for your chatbots on top of PHPUnit. BotMan Studio will automatically include example tests for you to look at. Take a look at the following test:

```php
public function testUserGreeting()
{
	$this->bot->receives('My name is Marcel')
			  ->assertQuestion('Hello Marcel. How old are you?')
			  ->receives('32')
			  ->assertReply('Got it - you are 32 years old, Marcel.');
}
```

For more information on testing, check out the <a href="">testing documentation</a>.

## Middleware
The concept of middleware has drastically improved with BotMan 2.0. Where 1.5 was very limited in the available entry points of your webhooks, 2.0 gives you the flexibility you need to extend the lifecircle of your chatbot application.

This also means, that you can now limit where in your chatbot you want to add middlewares. 
The available entry points are: `sending`, `received`, `heard`. 

Since these entrypoints are not dependenat on your command, they will be applied to all messages.

```php
/**
 * Middleware will be applied when your chatbot sends out messages.
 */
$this->botman->middleware->sending(new SendingMiddleware());

/**
 * Middleware will be applied when your chatbot receives incoming messages.
 */
$this->botman->middleware->received(new ReceivedMiddleware());

/**
 * Middleware will be applied when your chatbot successfully heard/matched a messages.
 */
$this->botman->middleware->heard(new HeardMiddleware());
```

For more information on middleware, check out the <a href="">middleware documentation</a>.

## Exception Handling
We have to admit it - exceptions in our code happen from time to time. But we need to catch them properly and notify the chatbot users if they happen. This becomes super simple with BotMan 2.0, as it provides a fluent and expressive API to hook into the exception handling during your chatbot lifetime.

```php
$botman->exception(Exception::class, function($exception, $bot) {
	$bot->reply('Sorry, something went wrong');
});
```

For more information on exception, check out the <a href="">exception documentation</a>.