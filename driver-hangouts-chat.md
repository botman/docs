# Hangouts Chat

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Hangouts Driver.

```sh
composer require botman/driver-hangouts
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Hangouts\HangoutsDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver hangouts
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with Hangouts Chat, create a bot and configure your HTTPS endpoint, as described in the [official documentation](https://developers.google.com/hangouts/chat/how-tos/bots-publish?authuser=1).
Take note of the verification token and place it in your Hangouts configuration.

If you use BotMan Studio, you can find the configuration file located under `config/botman/hangouts.php`.

If you dont use BotMan Studio, add these line to $config array that you pass when you create the object from BotManFactory.

```php
'hangouts' => [
	'token' => 'YOUR-WEBHOOK-TOKEN',
]
```


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
		<td>✅</td>
	</tr>
	<tr>
		<td>Image Attachment</td>
		<td>✅</td>
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