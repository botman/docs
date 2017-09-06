# Facebook Messenger

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)
- [Sending Facebook Templates](#sending-facebook-templates)
- [Supported Events](#supported-events)
- [Studio Features](#studio-features)
    - [Get Started Command](#get-started-command)
    - [Greeting Text Command](#greeting-text-command)
    - [Persistent Menu Command](#persistent-menu-command)
    - [Whitelist Domains Command](#whitelist-domains-command)

Facebook Messenger is the biggest Messenger out there and therefor a great choice for building a chatbot. There are more than 1 Billion active users per month.

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Facebook Driver.

```sh
composer require botman/driver-facebook
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Facebook\FacebookDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver facebook
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with Facebook Messenger, you first need to follow the [official quick start guide](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) to create your Facebook Messenger application and retrieve an access token as well as an app secret. Place both of them in your BotMan configuration. If you use BotMan Studio, you can find the configuration file located under `config/botman/facebook.php`.

```php
'botman' => [
	'facebook' => [
		'token' => 'YOUR-FACEBOOK-PAGE-TOKEN-HERE',
		'app_secret' => 'YOUR-FACEBOOK-APP-SECRET-HERE',
	]
],
```

After that you can setup the webhook, which connects the Facebook application with your BotMan application. This is covered in the above mentioned Quick Start Guide as well, as connecting your Facebook application to a Facebook page.


<a id="supported-features"></a>
## Supported Features
This is a list of features that the this driver supports.
If a driver does not support a specific action, it is in most cases a limitation from the messaging service - not BotMan.

<table class="table">
<thead>
	<tr>
		<th>Feature</th>
		<th>Supported?</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>Question-Buttons</td>
		<td>✅</td>
	</tr>
	<tr>
		<td>Image Attachment</td>
		<td>✅</td>
	</tr>
	<tr>
		<td>Video Attachment</td>
		<td>✅</td>
	</tr>
	<tr>
		<td>Audio Attachment</td>
		<td>✅</td>
	</tr>
	<tr>
		<td>Location Attachment</td>
		<td>✅</td>
	</tr>
</tbody>
</table>

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

<a id="supported-events"></a>
## Supported Events

The BotMan Facebook driver supports listening for the following events:
```
- messaging_checkout_updates
- messaging_deliveries
- messaging_optins
- messaging_reads
- messaging_referrals
```

<a id="studio-features"></a>
## Studio Features

<a id="get-started-command"></a>
### Get Started Command

Adding a [Get Started](https://developers.facebook.com/docs/messenger-platform/messenger-profile/get-started-button) button resolves the issue of users not knowing what to write to break the ice with your bot. It is displayed the first time the user interacts with a Facebook chatbot. When you click it, it will send a payload (text) to BotMan and you can react to it and send the first welcome message to the user and tell him how to use your bot. In order to define this payload you need to send a CURL with some data to Facebook. But BotMan Studio can do that for you too!

First define the payload text in your `config/botman/facebook.php` file.

```php
'start_button_payload' => 'YOUR_PAYLOAD_TEXT'
```

Then run the artisan command:
```sh
php artisan botman:facebookAddStartButton 
```

This will add the Get Started button to your page's chat. You are now able to listen to this button with the payload in your `hears` method.

```php
$botman->hears('YOUR_PAYLOAD_TEXT', function (BotMan $bot) {
	...
});
```

<a id="greeting-text-command"></a>
### Greeting Text Command

The [Facebook Greeting](https://developers.facebook.com/docs/messenger-platform/messenger-profile/greeting-text) text is shown on the welcome screen when a user interacts with your bot the first time. (like the Get Started Button)

Define this text in your `config/botman/facebook.php` file. Then use the Artisan command to trigger the command:

```sh
php artisan botman:facebookAddGreetingText
```

<a id="persistent-menu-command"></a>
### Persistent Menu Command

With BotMan Studio it now gets much easier to add a [Persistent Facebook Menu](https://developers.facebook.com/docs/messenger-platform/messenger-profile/persistent-menu) to your bot. First define the structure and types of your menu in your `config/botman/facebook.php` file. There you will find a `persistent_menu` demo menu payload. Just edit it to your needs.

Then use the Artisan command to trigger the command:

```sh
php artisan botman:facebookAddMenu
```

<a id="whitelist-domains-command"></a>
### Whitelist Domains Command

Some features like Messenger Extensions and Checkbox Plugin require a bot to specify a [domain whitelist](https://developers.facebook.com/docs/messenger-platform/messenger-profile/domain-whitelisting).

Define all domains in your `config/botman/facebook.php` file. Then use the Artisan command to trigger the command:

```sh
php artisan botman:facebookWhitelistDomains
```


