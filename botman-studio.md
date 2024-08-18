# BotMan Studio

BotMan Studio is an old project to make BotMan easier to work with using Laravel. 


However, it is simpler just to have a normal BotMan instance.. inside the Laravel shell manually. 
As a result, this part of BotMan has been deprecated.

# Laravel Installation

This example uses Telegram as an example. Modify these instructions to choose your preferred driver as needed. 

### Install Laravel

```
composer create-project laravel/laravel botman-demo
```
###  Install Botman

```
cd botman-demo
composer require botman/botman botman/driver-telegram
```
###  Configure Botman

```
mkdir config\botman
copy vendor\botman\botman\assets\config.php config\botman
copy vendor\botman\driver-telegram\stubs\telegram.php config\botman
```
_Remember to setup the Telegram token in app\config\telegram.php_

###  Add in app\routes\web.php

```php
Route::any('telegram', [App\Http\Controllers\TelegramController::class, 'index'])->name('telegram');
```

###  Ignore CSRF token in app\Http\Middleware\VerifyCsrfToken.php

```php
    protected $except = [
        '/telegram',
    ];
```

### Create file app\Http\Controllers\TelegramController.php

```php
<?php

namespace App\Http\Controllers;

use BotMan\BotMan\BotMan;
use BotMan\BotMan\BotManFactory;
use BotMan\BotMan\Cache\LaravelCache;
use BotMan\BotMan\Drivers\DriverManager;
use BotMan\Drivers\Telegram\TelegramContactDriver;
use BotMan\Drivers\Telegram\TelegramDriver;
use BotMan\Drivers\Telegram\TelegramLocationDriver;
use BotMan\Drivers\Telegram\TelegramPhotoDriver;
use Illuminate\Http\Request;

class TelegramController extends Controller
{
    public function index(Request $request)
    {
        DriverManager::loadDriver(TelegramDriver::class);
        DriverManager::loadDriver(TelegramPhotoDriver::class);
        DriverManager::loadDriver(TelegramLocationDriver::class);
        DriverManager::loadDriver(TelegramContactDriver::class);

        $config = array_merge([
            'config' => config('botman.config')
        ], [
            'web' => config('botman.web', [])
        ]);

        $botman = BotManFactory::create($config, new LaravelCache);

        $botman->hears('Hi', function (BotMan $bot) {
            $bot->reply('Hello!');
        });

        $botman->listen();
    }
}
```

_Originally posted by @filippotoso in https://github.com/botman/botman/discussions/1308#discussioncomment-3008604_

