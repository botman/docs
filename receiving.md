# Hearing Messages

- [Basic commands](#basic-commands)
- [Command parameters](#command-parameters)
- [Matching Regular Expression](#matching-regular-expressions)
- [Command groups](#command-groups)
    - [Drivers](#command-groups-drivers)
    - [Middleware](#command-groups-middleware)
    - [Channels](#command-groups-channels)
- [Middleware](#middleware)
- [Driver Specifics](#driver-specifics)

<a id="basic-commands"></a>
## Basic commands

The easiest way to listen for BotMan commands is by using listening for a specific keyword and a Closure, providing a very simple and expressive method of defining bot commands:

```php
$botman->hears('foo', function ($bot) {
    $bot->reply('Hello World');
});
```

In addition to the Closure you can also pass a class and method that will get called if the keyword matches.

```php
$botman->hears('foo', 'Some\Namespace\MyBotCommands@handleFoo');

class MyBotCommands {

    public function handleFoo($bot) {
        $bot->reply('Hello World');
    }

}
```

<a id="command-parameters"></a>
## Command Parameters

Listening to basic keywords is fine, but sometimes you will need to capture parts of the information your bot users are providing. 
For example, you may need to listen for a bot command that captures a user's name. You may do so by defining command parameters:

```php
$botman->hears('call me {name}', function ($bot, $name) {
    $bot->reply('Your name is: '.$name);
});
```

You may define as many command parameters as required by your command:

```php
$botman->hears('call me {name} the {adjective}', function ($bot, $name, $adjective) {
    //
});
```

Command parameters are always encased within `{}` braces and should consist of alphabetic characters.

<a id="matching-regular-expressions"></a>
## Matching Regular Expressions

If command parameters do not give you enough power and flexibility for your bot commands, you can also use more complex regular expressions in your bot commands. For example, you may want your bot to only listen for digits. You may do so by defining a regular expression in your command like this:


```php
$botman->hears('I want ([0-9]+)', function ($bot, $number) {
    $bot->reply('You will get: '.$number);
});
```

Just like the command parameters, each regular expression match group will be passed to the handling Closure.

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
});
```

The current Wit.ai middleware will send all incoming text messages to wit.ai and adds the `entities` received from wit.ai back to the message.
You can then access the information using `$bot->getMessage()->getExtras()`. This method returns an array containing all wit.ai entities.

If you only want to get a single element of the extras, you can optionally pass a key to the `getExtras` method. If no matching key was found, the method will return `null`.

In addition to that, it will check against a custom trait entity called `intent` instead of using the built-in matching pattern.

<a id="driver-specifics"></a>
## Driver Specifics

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
