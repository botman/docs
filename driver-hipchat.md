# HipChat

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the HipChat Driver.

```sh
composer require botman/driver-hipchat
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\HipChat\HipChatDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver hipchat
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with your HipChat team, you need to create an integration in the room(s) you want your bot to be in.
After you have created the integration, take note of the URL HipChat presents you at "Send messages to this room by posting to this URL". 

This URL will be used to send the BotMan replies to your rooms.
 
 > Note: If you want your bot to live in multiple channels, you need to add the integration multiple times. This is a HipChat limitation.
 
Next, you need to add webhooks to each channel that you have added an integration to.

The easiest way to do this is:

1. As a HipChat Administrator, go to `https://YOUR-HIPCHAT-TEAM.hipchat.com/account/api` and create an API token that has the "Administer Room" scope.
2. With this token, perform a `POST` request against the API to create a webhook:

```bash
curl -X POST -H "Authorization: Bearer YOUR-API-TOKEN" \
-H "Content-Type: application/json" \
-d '{
	"url": "https://MY-BOTMAN-CONTROLLER-URL/",
	"event": "room_message"
}' \
"https://botmancave.hipchat.com/v2/room/YOUR-ROOM-ID/webhook"
```
> If you want your bot to live in multiple channels, you need to add the webhook to each channel. This is a HipChat limitation.

Once you've set up the integration(s) and webhook(s), add them to your BotMan configuration.	

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
