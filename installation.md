# Installation

- [Server Requirements](#requirements)
- [Installing BotMan](#installation)
- [Basic Usage without BotMan Studio](#basic-usage-without-botman-studio)

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, including Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger, WeChat and many more.

<a id="installation"></a>
## Installing BotMan

BotMan utilizes [Composer](https://getcomposer.org/) to manage its dependencies. So, before using BotMan, make sure you have Composer installed on your machine.
<br><br>
The recommended way of getting started with BotMan and chatbot development is by using [BotMan Studio](/__version__/botman-studio) - a boilerplate project built on top of the popular Laravel PHP framework.

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
composer require botman/botman
```

This will download the BotMan package and all it's dependencies.

<a id="basic-usage-without-botman-studio"></a>
## Basic Usage without BotMan Studio

This code shows you the very basics of how you can use BotMan as a standalone composer dependency.
Make sure to place this code in a file that is accessible through the web - like a controller class.

```php
<?php

use BotMan\BotMan\BotMan;
use BotMan\BotMan\BotManFactory;

$config = [
    // Your driver-specific configuration
];

// create an instance
$botman = BotManFactory::create($configuration);

// give the bot something to listen for.
$botman->hears('hello', function (BotMan $bot) {
    $bot->reply('Hello yourself.');
});

// start listening
$botman->listen();
```

Pretty simple, right? To dig deeper, you may want to look into the core concept of [hearing messages](__version__/receiving) next.