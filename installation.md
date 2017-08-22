# Installation

- [Server Requirements](#requirements)
- [Installing BotMan](#installation)
- [Basic Usage without BotMan Studio](#basic-usage)

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, including Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger, WeChat and many more.

<a id="installation"></a>
## Installing BotMan

BotMan utilizes [Composer](https://getcomposer.org/) to manage its dependencies. So, before using BotMan, make sure you have Composer installed on your machine.
<br><br>
The recommended way of getting started with BotMan and chatbot development is by using [BotMan Studio](/__version__/botman-studio).

### Via BotMan Studio
First, download the BotMan installer using Composer:

```sh
composer global require "botman/installer"
```

Make sure to place the `$HOME/.composer/vendor/bin` directory (or the equivalent directory for your OS) in your $PATH so the `botman` executable can be located by your system.
<br><br>
Once installed, the `botman new` command will create a fresh BotMan Studio installation in the directory you specify. For instance, `botman new weatherbot` will create a directory named `weatherbot` containing a fresh BotMan Studio installation with all of BotMan's dependencies already installed:

```sh
botman new weatherbot
```

Take a look at the [BotMan Studio](/__version__/botman-studio) documentation, to learn more about how to add and configure messaging drivers.

### Via Composer

If you don't want to use [BotMan Studio](/__version__/botman-studio), you can install BotMan directly by requiring it in your existing project.

```sh
composer require mpociot/botman
```

<a id="basic-usage"></a>
## Basic Usage without BotMan Studio

This sample bot listens for the word "hello".
It listens on all messaging services, that you have configured - all using the exact same codebase.

```php
<?php

use Mpociot\BotMan\BotManFactory;
use Mpociot\BotMan\BotMan;

$config = [
    'hipchat_urls' => [
        'YOUR-INTEGRATION-URL-1',
        'YOUR-INTEGRATION-URL-2',
    ],
    'nexmo_key' => 'YOUR-NEXMO-APP-KEY',
    'nexmo_secret' => 'YOUR-NEXMO-APP-SECRET',
    'microsoft_bot_handle' => 'YOUR-MICROSOFT-BOT-HANDLE',
    'microsoft_app_id' => 'YOUR-MICROSOFT-APP-ID',
    'microsoft_app_key' => 'YOUR-MICROSOFT-APP-KEY',
    'slack_token' => 'YOUR-SLACK-TOKEN-HERE',
    'telegram_token' => 'YOUR-TELEGRAM-TOKEN-HERE',
    'facebook_token' => 'YOUR-FACEBOOK-TOKEN-HERE',
    'facebook_app_secret' => 'YOUR-FACEBOOK-APP-SECRET-HERE',
    'wechat_app_id' => 'YOUR-WECHAT-APP-ID',
    'wechat_app_key' => 'YOUR-WECHAT-APP-KEY',
];

// create an instance
$botman = BotManFactory::create($config);

// give the bot something to listen for.
$botman->hears('hello', function (BotMan $bot) {
    $bot->reply('Hello yourself.');
});

// start listening
$botman->listen();
```