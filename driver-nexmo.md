# Nexmo

- [Installation & Setup](#installation-setup)
- [Register Your Webhook](#register-webhook)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Nexmo Driver.

```sh
composer require botman/driver-nexmo
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Nexmo\NexmoDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver nexmo
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with Nexmo, you first need to create a Nexmo account [here](https://dashboard.nexmo.com/sign-up) and [buy a phone number](https://dashboard.nexmo.com/buy-numbers), which is capable of sending SMS.
<br><br>
Go to the Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings) and copy your API key and API secret into your BotMan configuration.
If you use BotMan Studio, you can find the configuration file located under `config/botman/nexmo.php`.

If you dont use BotMan Studio, add these line to $config array that you pass when you create the object from BotManFactory.

```php
'nexmo' => [
	'key' => 'YOUR-NEXMO-APP-KEY',
	'nexmo_secret' => 'YOUR-NEXMO-APP-SECRET',
]
```

<a id="register-webhook"></a>
## Register Your Webhook

To let Nexmo send your bot notifications when incoming SMS arrive at your numbers, you have to register the URL where BotMan is running at,
with Nexmo.

You can do this by visiting your Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings).

There you will find an input field called `Callback URL for Inbound Message` - place the URL that points to your BotMan logic / controller in this field.


<a id="supported-features"></a>
## Supported Features
This is a list of features that the driver supports.
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
		<td>❌</td>
	</tr>
	<tr>
		<td>Image Attachment</td>
		<td>❌</td>
	</tr>
	<tr>
		<td>Video Attachment</td>
		<td>❌</td>
	</tr>
	<tr>
		<td>Audio Attachment</td>
		<td>❌</td>
	</tr>
	<tr>
		<td>Location Attachment</td>
		<td>❌</td>
	</tr>
</tbody>
</table>