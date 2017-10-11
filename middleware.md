# Middleware

- [Introduction](#introduction)
- [Lifecycle](#lifecycle)
- [Received Middleware](#received-middleware)
- [Captured Middleware](#captured-middleware)
- [Matching Middleware](#matching-middleware)
- [Heard Middleware](#heard-middleware)
- [Sending Middleware](#sending-middleware)
- [Example](#middleware-example)

<a id="introduction"></a>
## Introduction
BotMan comes with a thorough middleware system, which is flexible and powerful. It allows you to hook custom integration logic into multiple parts of the chatbot lifecycle, such as receiving messages, matching message patterns and sending messages back to the user.

<a id="lifecycle"></a>
## Lifecycle

This chart shows you the lifecycle of the incoming chatbot requests and each steps they go through:

<div class="columns">
	<div class="column is-8">
		<img src="/img/middleware/lifecycle.png" />
	</div>
</div>

<a id="received-middleware"></a>
## Received Middleware
The `received` middleware can be used to manipulate all incoming messaging service requests. This can for example be used to pre-process the incoming data and send it to a natural language processing tool, such as API.ai or RASA NLU. Since the `received` middleware needs to be applied to all incoming requests, it needs to be defined globally.

```php
$middleware = new Middleware();
$botman->middleware->received($middleware);
```

<a id="captured-middleware"></a>
## Captured Middleware
The `captured` middleware processes incoming answers when the current user is inside a conversation flow. This middleware can be used to process the given answers in a conversation.
Since the `captured` middleware needs to be applied to all incoming requests, it needs to be defined globally.

```php
$middleware = new Middleware();
$botman->middleware->captured($middleware);
```

<a id="matching-middleware"></a>
## Matching Middleware
The `matching` middleware defines how the messages will be matched. It receives the result of the regular expression check and can perform additional checks too. This method is useful when testing incoming messages against natural language processing results - such as intents. This middleware can be applied per message that should be heard from BotMan.

```php
$middleware = new Middleware();
$botman->hears('keyword', function ($bot) {
	//
})->middleware($middleware);
```

<a id="heard-middleware"></a>
## Heard Middleware
The `heard` middleware works similar to how the `received` middleware works. It can process all incoming messages that were successfully heard.

```php
$middleware = new Middleware();
$botman->middleware->heard($middleware);
```

<a id="sending-middleware"></a>
## Sending Middleware
The `sending` middleware gets called before each message is sent out to the recipient. You can use this middleware to process outgoing messages and track every message that you send, for example using an analytics service.

```php
$middleware = new Middleware();
$botman->middleware->sending($middleware);
```
<a id="middleware-example"></a>
## Example
The following is a simple example of how to implement your custom middleware.

```php
class MyCustomMiddleware implements MiddlewareInterface
{
    /**
     * Handle a captured message.
     *
     * @param \BotMan\BotMan\Messages\Incoming\IncomingMessage $message
     * @param BotMan $bot
     * @param $next
     *
     * @return mixed
     */
    public function captured(IncomingMessage $message, $next, BotMan $bot)
    {
        return $next($message);
    }
    
    /**
     * Handle an incoming message.
     *
     * @param IncomingMessage $message
     * @param BotMan $bot
     * @param $next
     *
     * @return mixed
     */
    public function received(IncomingMessage $message, $next, BotMan $bot)
    {
        return $next($message);
    }
    
    /**
     * @param \BotMan\BotMan\Messages\Incoming\IncomingMessage $message
     * @param string $pattern
     * @param bool $regexMatched Indicator if the regular expression was matched too
     * @return bool
     */
    public function matching(IncomingMessage $message, $pattern, $regexMatched)
    {
        return true;
    }
    
    /**
     * Handle a message that was successfully heard, but not processed yet.
     *
     * @param \BotMan\BotMan\Messages\Incoming\IncomingMessage $message
     * @param BotMan $bot
     * @param $next
     *
     * @return mixed
     */
    public function heard(IncomingMessage $message, $next, BotMan $bot)
    {
        return $next($message);
    }
    
    /**
     * Handle an outgoing message payload before/after it
     * hits the message service.
     *
     * @param mixed $payload
     * @param BotMan $bot
     * @param $next
     *
     * @return mixed
     */
    public function sending($payload, $next, BotMan $bot)
    {
        return $next($payload);
    }
}
```

Let's consider a simple use case - suppose you want to show a typing indicator as soon as your bot receives any message:

```php
    public function received(IncomingMessage $message, $next, BotMan $bot)
    {
    	$bot->getDriver()->types($message);
        return $next($message);
    }
```

Note that we didn't use `$bot->types()` as you'd usually do because, at this point, BotMan has not yet determined which driver to use. 
The same applies to the `getUser()` function:

```php
    public function received(IncomingMessage $message, $next, BotMan $bot)
    {
    	$user = $bot->getDriver()->getUser($message);
	    // do something with the $user, i.e. store it to DB
        return $next($message);
    }
```
