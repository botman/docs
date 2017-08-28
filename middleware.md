# Middleware

- [Introduction](#introduction)
- [Lifecycle](#lifecycle)
- [Received Middleware](#received-middleware)
- [Captured Middleware](#captured-middleware)
- [Matching Middleware](#matching-middleware)
- [Heard Middleware](#heard-middleware)
- [Sending Middleware](#sending-middleware)

<a id="introduction"></a>
## Introduction
BotMan comes with a thorough middleware system, which is flexible and powerful. It allows you to hook custom integration logic into multiple parts of the chatbot lifecycle, such as receiving messages, matching message patterns and sending messages back to the user.

<a id="lifecycle"></a>
## Lifecycle

This charts shows you the lifecycle of the incoming chatbot requests and each steps they go through.

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
The `matching` middleware defines how the messages will be matched. It receives the result of the regular epxression check and can perform additional checks too. This method is useful when testing incoming messages against natural language processing results - such as intents. This middleware can be applied per message that should be heard from BotMan.

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

