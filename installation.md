# Installation

- [Server Requirements](#requirements)
- [Getting Started](#getting-started)
- [Installing BotMan](#installation)
- [Basic Usage without BotMan Studio](#basic-usage-without-botman-studio)

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, including Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger, WeChat and many more. If you run into any problems while installing, please let us know and join the [Developer Slack community](https://slack.botman.io).

> {callout-video} Want to get a headstart into chatbot development? Take a look at the [Build A Chatbot](https://buildachatbot.io) video course.

<a id="requirements"></a>
## Technical Requirements

- PHP >= 7.1.3
- For BotMan Studio checkout [Laravel's requirements](https://laravel.com/docs/5.5/installation#server-requirements)

<a id="getting-started"></a>
## Getting Started

We highly recommend using the BotMan Studio installer, which will give you BotMan on top of a fresh Larvel application. This also gives you advanced development tools, like out-of-box chatbot testing support and CLI tools to quickly create new conversations and middleware classes. If you do not want to use BotMan Studio, you can find some alternatives below.

<a id="studio-installation"></a>
## 1. BotMan Studio Installation

To use BotMan studio in the most efficient way, install the BotMan Studio installer globally with:

```sh
composer global require "botman/installer"
```

Make sure to place the `$HOME/.composer/vendor/bin` directory (or the equivalent directory for your OS) in your $PATH so the `botman` executable can be located by your system.

## 2. Create a new BotMan Project

You can create a new BotMan project into a new directory with the following command:

```sh
botman new <directory>
```

Alternatively, you may also create a new BotMan project by issuing the Composer create-project command in your terminal:

```sh
composer create-project --prefer-dist botman/studio <directory>
```

This will create a new folder, clone the [BotMan Studio application](https://github.com/botman/studio) and install all the necessary dependencies so that you can start working on your chatbot right away.

## 3. Testing your Chatbot

After a successfull installation, you can instantly try out your chatbot application. Go into the directory that you created and use the following command, to start a simple PHP server:

```sh
php artisan serve
```

You should see something like: `Laravel development server started: <http://127.0.0.1:8000>`.

Next, you can visit this URL and click on the `Tinker` link, which will give you a textinput where you can immediately interact with your bot.

Try it out, by typing `hi` or `start conversation`.

You can dig into the logic of this, by looking into the `routes/botman.php` file and take a look at the [hearing messages](/__version__/receiving) documentation next.

# Alternatives

To use BotMan without the use of BotMan Studio/Laravel, please see below:

<a id="installation"></a>
## Installing BotMan

BotMan utilizes [Composer](https://getcomposer.org/) to manage its dependencies. So, before using BotMan, make sure you have Composer installed on your machine.

```sh
composer require botman/botman
```

This will download the BotMan package and all its dependencies.

<a id="basic-usage-without-botman-studio"></a>
## Basic Usage without BotMan Studio

This code shows you the very basics of using BotMan as a standalone composer dependency.
Make sure to place this code in a file that is accessible through the web - like a controller class.

```php
<?php

use BotMan\BotMan\BotMan;
use BotMan\BotMan\BotManFactory;
use BotMan\BotMan\Drivers\DriverManager;

$config = [
    // Your driver-specific configuration
    // "telegram" => [
    //    "token" => "TOKEN"
    // ]
];

// Load the driver(s) you want to use
DriverManager::loadDriver(\BotMan\Drivers\Telegram\TelegramDriver::class);

// Create an instance
$botman = BotManFactory::create($config);

// Give the bot something to listen for.
$botman->hears('hello', function (BotMan $bot) {
    $bot->reply('Hello yourself.');
});

// Start listening
$botman->listen();
```

Pretty simple, right? To dig deeper, you may want to look into the core concept of [hearing messages](/__version__/receiving) next.
