# Installation

- [Installation & Setup](#installation-setup)
- [Basic Usage](#basic-usage)
- [Cache Drivers](#cache-drivers)

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, including Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger and WeChat.

```php
$botman->hears('I want cross-platform bots with PHP!', function (BotMan $bot) {
    $bot->reply('Look no further!');
});
```

<a id="installation-setup"></a>
## Installation & Setup

Require this package with composer using the following command:

```sh
$ composer require mpociot/botman
```

<a id="basic-usage"></a>
## Basic Usage

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

<a id="cache-drivers"></a>
## Cache drivers

If you want to make use of BotMans Conversation feature, you need to use a persistent cache driver, where BotMan can store and retrieve the conversations.
If not specified otherwise, BotMan will use ``array`` cache which is non-persistent. When using the Laravel facade it will automatically use the Laravel Cache component.

BotMan supports many cache drivers out of the box.

<a id="doctrine-cache"></a>
### Doctrine Cache
Use any [Doctrine Cache](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/caching.html) driver by passing it to the factory:

```php
use Mpociot\BotMan\Cache\DoctrineCache;

$botman = BotManFactory::create($config, new DoctrineCache($doctrineCacheDriver));
```

<a id="codeigniter-cache"></a>
### CodeIgniter Cache
Use any [CodeIgniter Cache](https://www.codeigniter.com/userguide3/libraries/caching.html) adapter by passing it to the factory:

```php
use Mpociot\BotMan\Cache\CodeIgniterCache;

$this->load->driver('cache');
$botman = BotManFactory::create($config, new CodeIgniterCache($this->cache->file));
```

<a id="redis-cache"></a>
### Redis Cache
[Redis](https://redis.io) in-memory data structure store. If you have https://github.com/igbinary/igbinary module available, it will be used instead of standard php serializer:

```php
use Mpociot\BotMan\Cache\RedisCache;

$botman = BotManFactory::create($config, new RedisCache('127.0.0.1', 6379));
```
