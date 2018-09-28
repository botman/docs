# Sending Messages

- [Introduction](#introduction)
- [Single Message Replies](#single-message-replies)
- [Attachments](#attachments)
- [Type Indicators](#type-indicators)
- [Originating Messages](#originating-messages)
- [Send Low-Level Requests](#sending-low-level-requests)

## Introduction

Bots send messages to deliver information and present an interface for their
functionality.  BotMan can send messages in several different ways, depending
on the type and number of messages that will be sent.

Single message replies to incoming commands can be sent using the `$bot->reply()` function.

Multi-message replies, particularly those that present questions for the end user to respond to,
are sent using the `$bot->startConversation()` function and the related [conversation](/__version__/conversations) sub-functions.

Bots can also originate messages - that means, sending messages based on some internal logic or external stimulus - using the `$bot->say()` method.

<a id="single-message-replies"></a>

## Single Message Replies

Once a bot has received a message using `hears()`, you may send a response using `$bot->reply()`.

This is the simplest way to respond to an incoming command:

```php
$botman->hears('single response', function (BotMan $bot) {
    $bot->reply("Tell me more!");
});
```
<a href="#" onclick="botmanChatWidget.say('single response');return false;" class="w-full block text-right font-bold">Try it out</a>

Do you feel like chatting some more? You may also send multiple responses in a single method.

```php
$botman->hears('multi response', function (BotMan $bot) {
    $bot->reply("Tell me more!");
    $bot->reply("And even more");
});
```
<a href="#" onclick="botmanChatWidget.say('multi response');return false;" class="w-full block text-right font-bold">Try it out</a>

<a id="attachments"></a>

## Attachments

A common use case would be including an attachment along with your message.
Depending on the messaging services that you are using, BotMan can send images, videos, audio files, generic files or even location data as attachment information.

You may do this by composing your message using the `OutgoingMessage` class. This class takes care of transforming your data for each
individual messaging service.

```php
use BotMan\BotMan\Messages\Attachments\Image;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

$botman->hears('image attachment', function (BotMan $bot) {
    // Create attachment
    $attachment = new Image('https://botman.io/img/logo.png');

    // Build message object
    $message = OutgoingMessage::create('This is my text')
                ->withAttachment($attachment);

    // Reply message object
    $bot->reply($message);
});
```
<a href="#" onclick="botmanChatWidget.say('image attachment');return false;" class="w-full block text-right font-bold">Try it out</a>

### Images
You may use the `Image` class to attach an image URL to your outgoing message.
It takes an image URL and an optional custom payload as constructor parameters,

```php
use BotMan\BotMan\Messages\Attachments\Image;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

// Create attachment
$attachment = new Image('http://image-url-here.jpg', [
    'custom_payload' => true,
]);

// Build message object
$message = OutgoingMessage::create('This is my text')
            ->withAttachment($attachment);

// Reply message object
$bot->reply($message);
```

### Videos
You may use the `Video` class to attach a video URL to your outgoing message.
It takes a video URL and an optional custom payload as constructor parameters.

```php
use BotMan\BotMan\Messages\Attachments\Video;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

// Create attachment
$attachment = new Video('http://video-url-here.mp4', [
    'custom_payload' => true,
]);

// Build message object
$message = OutgoingMessage::create('This is my text')
            ->withAttachment($attachment);

// Reply message object
$bot->reply($message);
```

### Audio
You may use the `Audio` class to attach an audio URL to your outgoing message.
It takes an audio URL and an optional custom payload as constructor parameters.

```php
use BotMan\BotMan\Messages\Attachments\Audio;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

// Create attachment
$attachment = new Audio('http://audio-url-here.mp3', [
    'custom_payload' => true,
]);

// Build message object
$message = OutgoingMessage::create('This is my text')
            ->withAttachment($attachment);

// Reply message object
$bot->reply($message);
```

### Generic Files
You may use the `File` class to attach a generic file URL to your outgoing message.
It takes a file URL and an optional custom payload as constructor parameters.

```php
use BotMan\BotMan\Messages\Attachments\File;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

// Create attachment
$attachment = new File('http://file-url-here.pdf', [
    'custom_payload' => true,
]);

// Build message object
$message = OutgoingMessage::create('This is my text')
            ->withAttachment($attachment);

// Reply message object
$bot->reply($message);
```

### Location Information
You may use the `Location` class to attach a GPS location information to your outgoing message.
It takes the latitude, longitude and an optional custom payload as constructor parameters.

```php
use BotMan\BotMan\Messages\Attachments\Location;
use BotMan\BotMan\Messages\Outgoing\OutgoingMessage;

// Create attachment
$attachment = new Location(61.766130, -6.822510, [
    'custom_payload' => true,
]);

// Build message object
$message = OutgoingMessage::create('This is my text')
            ->withAttachment($attachment);

// Reply message object
$bot->reply($message);
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

The easiest way is to just specify the driver-specific recipient ID when calling the `say` method and the driver to use.

```php
$botman->say('Message', 'my-recipient-user-id', TelegramDriver::class);
```

Just as the regular `reply` method, this method also accepts either simple strings or `OutgoingMessage` objects.

For BotFramework (Skype) you shoud pass `['serviceUrl' => 'https://smba.trafficmanager.net/apis/']` in fourth parameter.


<a id="sending-low-level-requests"></a>
## Sending Low-Level Requests

Sometimes you develop your chatbot and come to the conclusion that you would need to call a particular native messaging service API. As BotMan tries to decouple as many APIs as possible, it is not possible to have all API methods for each messaging service in the core of BotMan.

For this exact reason, there is the `sendRequest` method on the BotMan instance. What it does is, that it calls the messaging service endpoint with the given arguments and returns a Response object.

```php
// Calling the sendSticker API for Telegram
$botman->hears('sticker', function($bot) {
	$bot->sendRequest('sendSticker', [
		'sticker' => '1234'
	])
});
```

If your API endpoint needs a Chat-ID, User-ID or Channel-ID that BotMan already knows, it will be passed along in the appropriate format for you, so you do not need to worry about it.
