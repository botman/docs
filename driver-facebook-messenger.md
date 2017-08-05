# Facebook Messenger

- [Installation & Setup](#installation-setup)
- [Sending Facebook Templates](#sending-facebook-templates)

Facebook Messenger is the biggest Messenger out there and therefor a great choice for building a chatbot. There are more than 1 Billion active users per month.

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Facebook Driver repository.

```sh
$ composer require botman/driver-facebook
```

Facebook Messenger requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with Facebook Messenger, you first need to follow the [official quick start guide](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) to create your Facebook Messenger application and retrieve an access token as well as an app secret. Place both of them in your BotMan configuration.

```php
'botman' => [
	'facebook' => [
		'token' => 'YOUR-FACEBOOK-PAGE-TOKEN-HERE',
		'app_secret' => 'YOUR-FACEBOOK-APP-SECRET-HERE',
	]
],
```

<a id="sending-facebook-templates"></a>
## Sending Facebook Templates

BotMan supports all the main [Facebook templates](https://developers.facebook.com/docs/messenger-platform/send-api-reference/templates) like `Button`, `Generic`, `List` and `Receipt`. All of them are available through an expressive and easy API.

> {callout-info} Facebook is still experimenting a lot with its Messenger features. This is why some of them behave differently on certain platforms. General it is easy to say that all of them work within the native Messenger on your phones. But e.g. the List Template cover image is not working inside the Facebook website chat and the online Messenger.

### Button Template

<img alt="Facebook Button Template Screenshot" src="/img/fb/template_buttons.png" width="500">

A `Button Template` is a text with several user input options. (buttons) The template uses `ElementButtons` which are different from the buttons you use for BotMan `Questions`. There are two types of ElementButtons. The default one is the `web_url` button which only needs an `url` next to the title. It links to an external website. Secondly we have `postback` buttons. They will trigger [Facebook postback actions](https://developers.facebook.com/docs/messenger-platform/webhook-reference/postback). They require the type `postback` and a `payload which is the text that Facebook will send to BotMan` when a user hits this button. In the callback you will be able to get the postback button's text with `$answer->getText()`.

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