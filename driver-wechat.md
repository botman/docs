# WeChat

- [Installation & Setup](#installation-setup)

> {callout-info} Please note: WeChat currently does not support interactive messages / question buttons.

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the WeChat Driver.

```sh
composer require botman/driver-wechat
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver wechat
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

Login to your [developer sandbox account](http://admin.wechat.com/debug/cgi-bin/sandbox?t=sandbox/login) and take note of your appId and appSecret. This is your test bot.

There is a section called "API Config" where you need to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in the URL field.

This URL needs to validate itself against WeChat. Choose a unique
verify token, which you can check against in your verify controller and place it in the Token field.

Pass the WeChat App ID and App Key to the `BotManFactory` upon initialization. If you use BotMan Studio, you can find the configuration file located under `config/botman/wechat.php`.

```php
[
    'wechat' => [
    	'app_id' => 'YOUR-WECHAT-APP-ID',
    	'app_key' => 'YOUR-WECHAT-APP-KEY',
		'verification' => 'YOUR-WECHAT-VERIFICATION-TOKEN',
    ]
]
```

Last you need to connect the test bot with your WeChat user account. Use the `Test account QR Code` from your sandbox page to add the bot to your contacts. And that's it - you can now use BotMan with WeChat to create interactive bots!
