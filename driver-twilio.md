# Twilio

- [Installation & Setup](#installation-setup)
- [Register Your Webhook](#register-webhook)
- [Incoming Phone Calls](#incoming-phone-calls)
- [TwiML Support](#twiml-support)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Twilio Driver.

```sh
composer require botman/driver-twilio
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Twilio\TwilioVoiceDriver::class);
DriverManager::loadDriver(\BotMan\Drivers\Twilio\TwilioMessageDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver twilio
```

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create a public URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.

To connect BotMan with Twilio, you first need to create a Twilio account [here](https://www.twilio.com/try-twilio) and [buy a phone number](https://www.twilio.com/console/phone-numbers/incoming), which is capable of sending SMS/MMS or making phone calls.

<a id="register-webhook"></a>
## Register Your Webhook

To let Twilio send your chatbot a notifications when you receive an incoming call or receive a SMS, you have to register the URL where BotMan is running at, with Twilio.

You can do this on the [configuration page of your phone numbers](https://www.twilio.com/console/phone-numbers/incoming).
<br><br>
If you want to make use of Twilio's voice services: Place your webhook URL in the input field: `A CALL COMES IN`.<br>
If you want to make use of Twilio's SMS/messaging services: Place your webhook URL in the input field: `A MESSAGE COMES IN`.

<a id="incoming-phone-calls"></a>
## Incoming Phone Calls

To let your chatbot react to incoming phone calls, you can listen for the `TwilioVoiceDriver::INCOMING_CALL` event.

```php
$botman->on(TwilioVoiceDriver::INCOMING_CALL, function($payload, $bot) {
    $bot->startConversation(new ExampleConversation);
});
```

<a id="twiml-support"></a>
## TwiML Support

If you want total control over the output of your Twilio based chatbot, you can also directly reply any [TwiML response](https://www.twilio.com/docs/api/twiml) using the official Twilio PHP SDK.
```php
$twiml = new Twiml();
$twiml->record(['timeout' => 10, 'transcribe' => 'true']);

$bot->reply($twiml);
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
        <td>❌</td>
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
        <td>❌</td>
    </tr>
</tbody>
</table>