# Microsoft Bot Framework / Skype

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Bot Framework Driver.

```sh
composer require botman/driver-botframework
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver botframework
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

Register a developer account with the [Bot Framework Developer Portal](https://dev.botframework.com/) and follow [this guide](https://docs.botframework.com/en-us/csharp/builder/sdkreference/gettingstarted.html#registering) to register your first bot with the Bot Framework.
<br><br>
When you set up your bot, you will be asked to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in this field.
<br><br>
Take note of your App ID and App Key assigned to your new bot and place it in your BotMan configuration.
If you use BotMan Studio, you can find the configuration file located under `config/botman/botframework.php`.

```php
'botman' => [
    'botframework' => [
    	'app_id' => 'YOUR-MICROSOFT-APP-ID',
    	'app_key' => 'YOUR-MICROSOFT-APP-KEY',
    ]
],
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