# Configuration

- [Facebook Messenger](#facebook-messenger)
- [HipChat](#hipchat)
- [Microsoft Bot Framework](#ms-bot-framework)
- [Nexmo](#nexmo)
- [Slack](#slack)
- [Telegram](#telegram)
- [WeChat](#wechat)

> {callout-info} Most messaging drivers require a valid URL that can be accessed from the internet in order to set up webhooks. If you are working with Laravel, you can use [Valet](https://laravel.com/docs/5.3/valet) to get an URL to your local development environment using `valet share`.
If you are not developing with Laravel, take a look at [Ngrok](http://ngrok.io) to create a sharable URL.

<a id="facebook-messenger"></a>
## Facebook Messenger

Facebook Messenger requires a valid and secure (HTTPS) URL in order to set up webhooks and receive events and information from the chat users. This URL needs to validate itself against Facebook. When you create the webhook on the Facebook developer website, you have to choose a unique verify token, which you can check against in your verify controller.
                                    
BotMan comes with a method to simplify the verification process.

Just place this line after the initialization:

```
$botman->verifyServices('MY_SECRET_VERIFICATION_TOKEN');
```

To connect BotMan with Facebook Messenger, you first need to follow the [official quick start guide](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) to create your Facebook Messenger application and retrieve an access token.

Once you have obtained the page access token, place it in your BotMan configuration.

If you want BotMan to automatically verify each incoming Facebook webhook, you can optionally place your app secret in the BotMan configuration array.

```php
'botman' => [
    'facebook_token' => 'YOUR-FACEBOOK-PAGE-TOKEN-HERE',
    'facebook_app_secret' => 'YOUR-FACEBOOK-APP-SECRET-HERE',
],
```

<a id="hipchat"></a>
## HipChat

> {callout-warning} Please note: HipChat currently does not support interactive messages / question buttons.

To connect BotMan with your HipChat team, you need to create an integration in the room(s) you want your bot to be in.
After you have created the integration, take note of the URL HipChat presents you at "Send messages to this room by posting to this URL". 

This URL will be used to send the BotMan replies to your rooms.
 
 > Note: If you want your bot to live in multiple channels, you need to add the ingration multiple times. This is a HipChat limitation.
 
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

<a id="ms-bot-framework"></a>
## Microsoft Bot Framework / Skype

Register a developer account with the [Bot Framework Developer Portal](https://dev.botframework.com/) and follow [this guide](https://docs.botframework.com/en-us/csharp/builder/sdkreference/gettingstarted.html#registering) to register your first bot with the Bot Framework.

When you set up your bot, you will be asked to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in this field.

Take note of your Bot handle, the App ID and App Key assigned to your new bot.

```php
'botman' => [
    'microsoft_bot_handle' => 'YOUR-MICROSOFT-BOT-HANDLE',
    'microsoft_app_id' => 'YOUR-MICROSOFT-APP-ID',
    'microsoft_app_key' => 'YOUR-MICROSOFT-APP-KEY',
],
```

<a id="nexmo"></a>
## Nexmo

To connect BotMan with Nexmo, you first need to create a Nexmo account [here](https://dashboard.nexmo.com/sign-up) and [buy a phone number](https://dashboard.nexmo.com/buy-numbers), which is capable of sending SMS.

Go to the Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings) and copy your API key and API secret.

```php
'botman' => [
    'nexmo_key' => 'YOUR-NEXMO-APP-KEY',
    'nexmo_secret' => 'YOUR-NEXMO-APP-SECRET',
],
```

### Register your Webhook

To let Nexmo send your bot notifications when incoming SMS arrive at your numbers, you have to register the URL where BotMan is running at,
with Nexmo.

You can do this by visiting your Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings).

There you will find an input field called `Callback URL for Inbound Message` - place the URL that points to your BotMan logic / controller in this field.

<a id="slack"></a>
## Slack

### Usage with the Realtime API

> {callout-warning} Please note: The Realtime API requires the additional composer package `mpociot/slack-client` to be installed.
> 
> Simply install it using `composer require mpociot/slack-client`.

Add a new Bot user to your Slack team and take note of the bot token slack gives you.
Use this token as your `slack_token` configuration parameter.

As the Realtime API needs a websocket, you need to create a PHP script that will hold your bot logic, as you can not use the HTTP controller way for it.

```php
<?php
require 'vendor/autoload.php';

use Mpociot\BotMan\BotManFactory;
use React\EventLoop\Factory;

$loop = Factory::create();
$botman = BotManFactory::createForRTM([
    'slack_token' => 'YOUR-SLACK-BOT-TOKEN'
], $loop);

$botman->hears('keyword', function($bot) {
    $bot->reply('I heard you! :)');
});

$botman->hears('convo', function($bot) {
    $bot->startConversation(new ExampleConversation());
});

$loop->run();
```

Then simply run this file by using `php my-bot-file.php` - your bot should connect to your Slack team and respond to the messages.

### Usage with an outgoing webhook

Add a new "Outgoing Webhook" integration to your Slack team - this URL needs to point to the controller where your BotMan bot is living in.

To let BotMan listen to all incoming messages, do **not** specify a trigger word, but define a channel instead.

With the Webhook implementation, there is no need to add a `slack_token` configuration. 
_Yes - that is all you need to do, to use BotMan_

<a id="telegram"></a>
## Telegram

To connect BotMan with your Telegram Bot, you first need to follow the [official guide](https://core.telegram.org/bots#3-how-do-i-create-a-bot) to create your Telegram Bot and an access token.

Once you have obtained the access token, place it in your BotMan configuration.

```php
'botman' => [
    'telegram_token' => 'YOUR-TELEGRAM-TOKEN-HERE',
],
```

### Register your Webhook

To let your Telegram Bot know, how it can communicate with your BotMan bot, you have to register the URL where BotMan is running at,
with Telegram.

You can do this by sending a `POST` request to this URL:

`https://api.telegram.org/bot<YOUR-TELEGRAM-TOKEN-HERE>/setWebhook`

This POST request needs only one parameter called `url` with the URL that points to your BotMan logic / controller.

<a id="wechat"></a>
## WeChat

> {callout-warning} Please note: WeChat currently does not support interactive messages / question buttons.

Login to your [developer sandbox account](http://admin.wechat.com/debug/cgi-bin/sandbox?t=sandbox/login) and take note of your appId and appSecret.

There is a section called "API Config" where you need to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in the URL field.

This URL needs to validate itself against WeChat. Choose a unique
verify token, which you can check against in your verify controller and place it in the Token field.

BotMan comes with a method to simplify the verification process. Just place this line after the initialization:

```php
$botman->verifyServices('', 'MY_SECRET_WECHAT_VERIFICATION_TOKEN');
```

Pass the WeChat App ID and App Key to the `BotManFactory` upon initialization.

```php
[
    'wechat_app_id' => 'YOUR-WECHAT-APP-ID',
    'wechat_app_key' => 'YOUR-WECHAT-APP-KEY',
]
```

And that's it - you can now use BotMan with WeChat to create interactive bots!
