# Telegram

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)
- [Register Your Webhook](#register-webhook)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Telegram Driver.

```sh
composer require botman/driver-telegram
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Telegram\TelegramDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver telegram
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.


To connect BotMan with your Telegram Bot, you first need to follow the [official guide](https://core.telegram.org/bots#3-how-do-i-create-a-bot) to create your Telegram Bot and an access token.

Once you have obtained the access token, place it in your BotMan configuration. If you use BotMan Studio, you can find the configuration file located under `config/botman/telegram.php`.

If you dont use BotMan Studio, add these line to $config array that you pass when you create the object from BotManFactory.

```php
'telegram' => [
	'token' => 'YOUR-TELEGRAM-TOKEN-HERE',
]
```


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

<a id="register-webhook"></a>
## Register Your Webhook

To let your Telegram Bot know, how it can communicate with your BotMan bot, you have to register the URL where BotMan is running at,
with Telegram.

You can do this by sending a `POST` request to this URL:

`https://api.telegram.org/bot<YOUR-TELEGRAM-TOKEN-HERE>/setWebhook`

This POST request needs only one parameter called `url` with the URL that points to your BotMan logic / controller.
If you use [BotMan Studio](/__version__/botman-studio) it will be:
`https://yourapp.domain/botman`. HTTPS is a must, because of security reasons.
