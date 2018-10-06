# Amazon Alexa

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)
- [Accessing Alexa Slots](#alexa-slots)
- [Sending Alexa Cards](#sending-alexa-cards)

The Amazon Alexa driver allows you to listen for pre-configured intents through the Alexa skill builder. You can also reply with custom Amazon Alexa Cards.

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Amazon Alexa Driver.

```sh
composer require botman/driver-amazon-alexa
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\AmazonAlexa\AmazonAlexaDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver amazon-alexa
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.


To connect BotMan with your Amazon Alexa skill, you first need to follow the [official guide](https://developer.amazon.com/docs/ask-overviews/build-skills-with-the-alexa-skills-kit.html) to create your custom Amazon Alexa skill.

Amazon Alexa does not require any kind of configuration.


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
        <td>Conversations</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Question-Buttons</td>
        <td>❌</td>
    </tr>
    <tr>
        <td>Image Attachment</td>
        <td>✅ - through Cards</td>
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

<a id="alexa-slots"></a>
## Accessing Alexa Slots

Amazon Alexa allows your intents to contain custom arguments - called "slots". Whenever you hear for a Alexa message that contains these slots, they will get stored as a `slot` extra on the IncomingMessage object. To access it, just retrieve it from the message extras:

```php
$botman->hears('My-Intent-Name', function($bot) {
    $slots = $bot->getMessage()->getExtras('slots');
});
```

<a id="sending-alexa-cards"></a>
## Sending Alexa Cards

Amazon Alexa allows your custom skill to reply not only by using voice, but also by adding custom [Skill Cards](https://developer.amazon.com/docs/custom-skills/include-a-card-in-your-skills-response.html) to your replies. These are graphical cards that describe or enhance the voice interaction.

To create and send such a Card with BotMan, just create a `Card` object and add it as an OutgoingMessage attachment like this:

```php
$botman->hears('My-Intent-Name', function($bot) {
    $card = Card::create('Your card title', 'Your card subtitle')
        ->type(Card::STANDARD_CARD_TYPE)
        ->image('https://botman.io/images/foo.png')
        ->text('This is a longer text for your card.');

    $message = OutgoingMessage::create('This is the spoken response')->withAttachment($card)
    $bot->reply($message);
});
```
