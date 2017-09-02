# BotMan Studio

- [Introduction](#introduction)
- [Installation](#installation)
- [Tinker](#tinker)
- [List All Drivers](#list-drivers)
- [Install New Drivers](#install-drivers)

## Introduction

While BotMan itself is framework agnostic, BotMan is also available as a bundle with the great [Laravel](http://laravel.com) PHP framework. This bundled version is called BotMan Studio and makes the chatbot development experience even better, by providing testing tools, an out of the box web driver implementation to get you started and additional tools like easier driver installation and configuration support.
If you are not yet familiar with Laravel, you may take a look at the Laracasts [free introduction to Laravel](https://laracasts.com/series/laravel-from-scratch-2017) video course.

<a id="installation"></a>
## Installation

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

Alternatively, you may also install BotMan Studio by issuing the Composer create-project command in your terminal:

```sh
composer create-project --prefer-dist botman/studio weatherbot
```

<a id="tinker"></a>
## Tinker

Chatbot development should be easy to get started with. Picking a messaging service and taking care of all the developer account pre-requisites is not.
That's why BotMan Studio comes with a ready-to-use BotMan web driver implementation.
This allows you to develop your chatbot locally and test it using your local BotMan Studio installation.
Your newly installed BotMan Studio project will contain a `/botman/tinker` route, where you will see a very simple VueJS based chat widget, that let's you communicate with your chatbot.
<br><br>
If you want to disable the built-in tinker route, just remove the line from your `routes/web.php` file:

```php
Route::get('/botman/tinker', 'BotManController@tinker');
```

If you want to modify the pre-installed VueJS component, you may modify the `BotManTinker.vue` file located at `resources/assets/js/components` in your BotMan Studio folder structure.

<a id="list-drivers"></a>
## List All Drivers

BotMan Studio makes Driver installation as easy as possible. To get an overview of all core drivers that are available and installed in your application, you can use the BotMan Studio artisan command:

```sh
php artisan botman:list-drivers
```

<a id="install-drivers"></a>
## Install New Drivers

Just like listing all available and installed drivers, you may also install new drivers using the `botman:install-driver` artisan command. It takes the driver name as an argument and will perform a `composer require` of the selected BotMan messaging driver.

So if you want to install the Facebook driver for BotMan, you can do this by using this artisan command:

```sh
php artisan botman:install-driver facebook
```

After composer has downloaded all the driver dependencies, a new configuration file will automatically be published inside your `config/botman` directory.