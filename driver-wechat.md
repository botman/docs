# WeChat

> {callout-warning} Please note: WeChat currently does not support interactive messages / question buttons.

Login to your [developer sandbox account](http://admin.wechat.com/debug/cgi-bin/sandbox?t=sandbox/login) and take note of your appId and appSecret. This is your test bot.

There is a section called "API Config" where you need to provide a public accessible endpoint for your bot.
Place the URL that points to your BotMan logic / controller in the URL field.

This URL needs to validate itself against WeChat. Choose a unique
verify token, which you can check against in your verify controller and place it in the Token field.

BotMan comes with a method to simplify the verification process. Just place this line after the initialization:

```php
// In your BotMan controller
$botman->verifyServices('', 'MY_SECRET_WECHAT_VERIFICATION_TOKEN');

// ...
$botman->hears('foo', function($bot){});
$botman->listen();
```

Pass the WeChat App ID and App Key to the `BotManFactory` upon initialization.

```php
[
    'wechat' => [
    	'app_id' => 'YOUR-WECHAT-APP-ID',
    	'app_key' => 'YOUR-WECHAT-APP-KEY',
    ]
]
```

Last you need to connect the test bot with your WeChat user account. Use the `Test account QR Code` from your sandbox page to add the bot to your contacts. And that's it - you can now use BotMan with WeChat to create interactive bots!
