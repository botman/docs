# Installation with Laravel


- [Project Starter Kit](#starter-kit)
- [Tinker](#tinker)
- [Use BotMan in your existing project](#existing-projects)

<a id="starter-kit"></a>
## Project Starter Kit

The fastest and easiest way to get your first bot up and running is by using the BotMan Laravel starter project.
You can create a new project using the `composer create-project` command:

```sh
$ composer create-project mpociot/botman-laravel-starter my_new_bot
```

This will create a new folder called `my_new_bot` with a fresh Laravel 5.4 project, which is preconfigured for the use of BotMan.
After you created the boiler plate project, edit your `.env` file with the configuration services of your choice.

The bot logic can be found in the `routes/botman.php` file, which works as a single definition point for your BotMan `hears` commands.

```php
$botman->hears('test', function($bot){
    $bot->reply('hello!');
});

$botman->hears('Start conversation', BotManController::class.'@startConversation');
```

<a id="tinker"></a>
## Tinker

The Laravel starter project comes with a neat Artisan command, that allows you to dry-run your chatbot without needing to setup external services.

Just run `php artisan botman:tinker` and you have a chat with your bot right in your terminal!

<a id="existing-projects"></a>
## Use BotMan in your existing project
Require this package with composer using the following command:

```sh
$ composer require mpociot/botman
```

BotMan comes with a Service Provider to make using this library in your [Laravel](http://laravel.com) application as simple as possible.

Go to your `config/app.php` and add the service provider:

```php
Mpociot\BotMan\BotManServiceProvider::class,
```

Also add the alias / facade:

```php
'BotMan' => Mpociot\BotMan\Facades\BotMan::class
```

Add your Facebook access token / Slack token to your `config/services.php`:

```php
'botman' => [
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
    'facebook_app_secret' => 'YOUR-FACEBOOK-APP-SECRET-HERE', // Optional - this is used to verify incoming API calls,
    'wechat_app_id' => 'YOUR-WECHAT-APP-ID',
    'wechat_app_key' => 'YOUR-WECHAT-APP-KEY',
],
```

Using it:

```php
<?php

use Mpociot\BotMan\BotMan;

$botman = app('botman');

$botman->hears('hello', function (BotMan $bot) {
    $bot->reply('Hello yourself.');
});

// start listening
$botman->listen();
```

Make sure that your controller method doesn't use the CSRF token middleware.

That's it.
