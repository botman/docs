# Microsoft Bot Framework / Skype

Register a developer account with the [Bot Framework Developer Portal](https://dev.botframework.com/) and follow [this guide](https://docs.botframework.com/en-us/csharp/builder/sdkreference/gettingstarted.html#registering) to register your first bot with the Bot Framework.

When you set up your bot, you will be asked to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in this field.

Take note of your Bot handle, the App ID and App Key assigned to your new bot.

```php
'botman' => [
    'botframework' => [
    	'bot_handle' => 'YOUR-MICROSOFT-BOT-HANDLE',
    	'app_id' => 'YOUR-MICROSOFT-APP-ID',
    	'app_key' => 'YOUR-MICROSOFT-APP-KEY',
    ]
],
```