# Sending Messages

- [Introduction](#introduction)
- [Single Message Replies](#single-message-replies)
- [Type Indicators](#type-indicators)
- [Originating Messages](#originating-messages)

## Introduction

Bots have to send messages to deliver information and present an interface for their
functionality.  BotMan can send messages in several different ways, depending
on the type and number of messages that will be sent.

Single message replies to incoming commands can be sent using the `$bot->reply()` function.

Multi-message replies, particularly those that present questions for the end user to respond to,
can be sent using the `$bot->startConversation()` function and the related [conversation](/conversations) sub-functions. 

Bots can originate messages - that is, send a message based on some internal logic or external stimulus - using `$bot->say()` method.

<a id="single-message-replies"></a>

## Single Message Replies

Once a bot has received a message using `hears()`, you may send a response  using `$bot->reply()`.

This is the simplest way to respond to an incoming command:

```php
$botman->hears('keyword', function (BotMan $bot) {
    $bot->reply("Tell me more!");
});
```

A common use case would be to not only send plain-text messages, but to also include images or videos.

You may do this by composing your message using the `Message` class. This class takes care of transforming your data for each
individual messaging service.

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

<a id="type-indicators"></a>

## Type Indicators

To make your bot feel and act more human, you can make it send "typing ..." indicators. 

```php

$botman->hears('keyword', function (BotMan $bot) {
    $bot->typesAndWaits(2);
    $bot->reply("Tell me more!");
});
```

This will send a typing indicator and sleep for 2 seconds, before actually sending the "Tell me more!" response.

Please note, that not all messaging services support typing indicators. If it is not supported, it will simply do nothing and just reply the message.

<a id="originating-messages"></a>

## Originating Messages

BotMan also allows you to send messages to your chat users programatically. You could, for example, send out a daily message to your users that get's triggered
by your cronjob.

The easiest way is to just specify the driver-specific recipient ID when calling the `say` method.

```php
$botman->say('Message', 'my-recipient-user-id');
```

You may also specify the messaging driver if you know it:

```php
$botman->say('Message', 'my-recipient-user-id', TelegramDriver::class);
```

Just as the regular `reply` method, this method also accepts either simple strings or `Message` objects.