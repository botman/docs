# Sending Messages

- [Introduction](#introduction)
- [Single Message Replies](#single-message-replies)
- [Type Indicators](#type-indicators)
- [Originating Messages](#originating-messages)
- [Sending Facebook Templates](#sending-facebook-templates)
- [Send Low-Level requests](#sending-low-level-requests)

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

For Slack Realtime API, you can upload a file like so:

```php
$botman->hears('give file to me', function (BotMan $bot) {
    // Build message object
    $message = Message::create('This is my comment for the uploaded file')
                ->filePath('/tmp/file.mp3');
    
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

<a id="sending-facebook-templates"></a>
## Sending Facebook Templates

BotMan supports all the main [Facebook templates](https://developers.facebook.com/docs/messenger-platform/send-api-reference/templates) like `Button`, `Generic`, `List` and the `Receipt`. All of them are available through an expressive and easy API.

> {callout-info} Facebook is still experimenting a lot with its Messenger features. This is why some of them behave differently on certain platforms. General it is easy to say that all of them work within the native Messenger on your phones. But e.g. the List Template cover image is not working inside the Facebook website chat and the online Messenger.

### Button Template

<img alt="Facebook Button Template Screenshot" src="/img/fb/template_buttons.png" width="500">

A `Button Template` is a text with several user input options. (buttons) The template uses `ElementButtons` which are different from the buttons you use for BotMan `Questions`. There are two types of ElementButtons. The default one is the `web_url` button which only needs an `url` next to the title. It links to an external website. Secondly we have `postback` buttons. They will trigger [Facebook postback actions](https://developers.facebook.com/docs/messenger-platform/webhook-reference/postback). They require the type `postback` and a `payload which is the text that Facebook will send to BotMan` when a user hits this button.

```php
$bot->reply(ButtonTemplate::create('Do you want to know more about BotMan?')
	->addButton(ElementButton::create('Tell me more')->type('postback')->payload('tellmemore'))
	->addButton(ElementButton::create('Show me the docs')->url('http://botman.io/'))
);
```

### Generic Template

<img alt="Facebook Generic Template Screenshot" src="/img/fb/template_generic.png" width="500">

A `Generic Template` is a horizontal scrollable carousel of elements. Every element requires at least a title which is provided through the static `create` method. Additionally it can have a `subtitle`, `image` and `buttons`.

```php
$bot->reply(GenericTemplate::create()
	->addImageAspectRatio(GenericTemplate::RATIO_SQUARE)
	->addElements([
		Element::create('BotMan Documentation')
			->subtitle('All about BotMan')
			->image('http://botman.io/img/botman-body.png')
			->addButton(ElementButton::create('visit')->url('http://botman.io'))
			->addButton(ElementButton::create('tell me more')
				->payload('tellmemore')->type('postback')),
		Element::create('BotMan Laravel Starter')
			->subtitle('This is the best way to start with Laravel and BotMan')
			->image('http://botman.io/img/botman-body.png')
			->addButton(ElementButton::create('visit')
				->url('https://github.com/mpociot/botman-laravel-starter')
			)
	])
);
```

### List Template

<img alt="Facebook List Template Screenshot" src="/img/fb/template_list.png" width="500">

The `List Template` is a template that allows you to present a set of elements vertically. The default list will set the first element as a cover image. If you don't want a cover image call the `useCompactView()` method. Additionally to the elements your list can have one global button too. This is when you need the `addGlobalButton(...)` method.

```php
$bot->reply(ListTemplate::create()
	->useCompactView()
	->addGlobalButton(ElementButton::create('view more')->url('http://test.at'))
	->addElement(
		Element::create('BotMan Documentation')
			->subtitle('All about BotMan')
			->image('http://botman.io/img/botman-body.png')
			->addButton(ElementButton::create('tell me more')
				->payload('tellmemore')->type('postback'))
	)
	->addElement(
		Element::create('BotMan Laravel Starter')
			->subtitle('This is the best way to start with Laravel and BotMan')
			->image('http://botman.io/img/botman-body.png')
			->addButton(ElementButton::create('visit')
				->url('https://github.com/mpociot/botman-laravel-starter')
			)
	)
);
```

### Receipt Template

<img alt="Facebook Receipt Template Screenshot" src="/img/fb/template_receipt.png" width="500">

Use the `Receipt Template` to send a order confirmation, with the transaction summary and description for each element. This template differs a lot from the others. This is why there are custom `ReceiptElements` and lots of other custom fields. Checkout the official [Facebook documentation](https://developers.facebook.com/docs/messenger-platform/send-api-reference/receipt-template) and the example below to see all the possibilities. In your Messenger you can click the template to see all of the information.

```php
$bot->reply(
	ReceiptTemplate::create()
		->recipientName('Christoph Rumpel')
		->merchantName('BotMan GmbH')
		->orderNumber('342343434343')
		->timestamp('1428444852')
		->orderUrl('http://test.at')
		->currency('USD')
		->paymentMethod('VISA')
		->addElement(ReceiptElement::create('T-Shirt Small')->price(15.99)->image('http://botman.io/img/botman-body.png'))
		->addElement(ReceiptElement::create('Sticker')->price(2.99)->image('http://botman.io/img/botman-body.png'))
		->addAddress(ReceiptAddress::create()
			->street1('Watsonstreet 12')
			->city('Bot City')
			->postalCode(100000)
			->state('Washington AI')
			->country('Botmanland')
		)
		->addSummary(ReceiptSummary::create()
			->subtotal(18.98)
			->shippingCost(10 )
			->totalTax(15)
			->totalCost(23.98)
		)
		->addAdjustment(ReceiptAdjustment::create('Laravel Bonus')->amount(5))
);
```

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
