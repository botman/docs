# Telegram

To connect BotMan with your Telegram Bot, you first need to follow the [official guide](https://core.telegram.org/bots#3-how-do-i-create-a-bot) to create your Telegram Bot and an access token.

Once you have obtained the access token, place it in your BotMan configuration.

```php
'botman' => [
    'telegram' => [
    	'token' => 'YOUR-TELEGRAM-TOKEN-HERE',
    ]
],
```

## Register your Webhook

To let your Telegram Bot know, how it can communicate with your BotMan bot, you have to register the URL where BotMan is running at,
with Telegram.

You can do this by sending a `POST` request to this URL:

`https://api.telegram.org/bot<YOUR-TELEGRAM-TOKEN-HERE>/setWebhook`

This POST request needs only one parameter called `url` with the URL that points to your BotMan logic / controller.
If you use [mpociot/botman-laravel-starter](https://github.com/mpociot/botman-laravel-starter) it will be:
`https://yourapp.domain/botman`. HTTPS is a must, because of security reasons.
