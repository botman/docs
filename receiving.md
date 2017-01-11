# Hearing Messages


- [Introduction](#introduction)
- [Matching Patterns](#matching-patterns)
- [Middleware](#middleware)

## Introduction

BotMan can listen to many different [messaging drivers](#connect-with-your-messaging-service) and therefore it might be required for you, to respond differently depending on which
driver was used to respond to your message.

Each messaging driver in BotMan has a `getName()` method, that returns a human readable name of the driver.
 
You can access the driver object using `$botman->getDriver()`.
To match against the driver name, you can use each driver's `NAME` constant or use the table below.

The available driver names are:

| Driver | Name 
|--- |---
| `BotFrameworkDriver` | BotFramework
| `FacebookDriver` | Facebook
| `HipChatDriver` | HipChat
| `NexmoDriver` | Nexmo
| `SlackDriver` | Slack
| `TelegramDriver` | Telegram
| `WeChatDriver` | WeChat

<a id="matching-patterns"></a>
## Matching Patterns and Keywords

BotMan provides a `hears()` function, which will listen to specific patterns in public and/or private channels.

| Argument | Description
|--- |---
| pattern | A string with a regular expressions to match
| callback | Callback function or `Classname@method` notation that receives a BotMan object, as well as additional matching regular expression parameters
| in | Defines where the Bot should listen for this message. Can be either `BotMan::DIRECT_MESSAGE` or `BotMan::PUBLIC_CHANNEL`.

<div class="col-xs-12 col-md-8">

```php
$botman->hears('keyword', function(BotMan $bot) {
    // do something to respond to message
    $bot->reply('You used a keyword!');
});

$botman->hears('keyword', 'MyClass@heardKeyword');
```

When using the built in regular expression matching, the results of the expression will be passed to the callback function. For example:

```php
$botman->hears('open the {doorType} doors', function(BotMan $bot, $doorType) {
    if ($doorType === 'pod bay') {
        return $bot->reply('I\'m sorry, Dave. I\'m afraid I can\'t do that.');
    }

    return $bot->reply('Okay');
});
```

</div>

<div class="hidden-xs col-md-4 bot-example">
    <iframe src="https://webchat.botframework.com/embed/botman-test-bot?s=hBI7tb3_Coc.cwA.u4Y.W4LWjA_6pOtpAqkSZ9Bwcfwv_e1OshFHjhpfUImhb8Q" border="none" style="border:none;height: 502px; max-height: 502px;"></iframe>
</div>

<a id="middleware"></a>
## Midleware

The usage of custom middleware allows you to enrich the messages your bot received with additional information from third party services such as [wit.ai](http://wit.ai) or [api.ai](http://api.ai).

To let your BotMan instance make use of a middleware, simply add it to the list of middlewares:

```php
$botman->middleware(Wit::create('MY-WIT-ACCESS-TOKEN'));
$botman->hears('emotion', function($bot) {
    $extras = $bot->getMessage()->getExtras();
    // Access extra information
    $entities = $extras['entities'];
    ...
});
```

The current Wit.ai middleware will send all incoming text messages to wit.ai and adds the `entities` received from wit.ai back to the message.
You can then access the information using `$bot->getMessage()->getExtras()`. This method returns an array containing all wit.ai entities. This extra information will help you building the next reply.

If you only want to get a single element of the extras, you can optionally pass a key to the `getExtras` method. If no matching key was found, the method will return `null`.

In addition to that, it will check against a custom trait entity called `intent` instead of using the built-in matching pattern.
