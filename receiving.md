# Hearing Messages

- [Basic commands](#basic-commands)
- [Command parameters](#command-parameters)
- [Matching Regular Expression](#matching-regular-expressions)
- [Command groups](#command-groups)
    - [Drivers](#command-groups-drivers)
    - [Middleware](#command-groups-middleware)
    - [Channels](#command-groups-channels)
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

<a id="command-groups"></a>
## Command Groups

Command groups allow you to share command attributes, such as middleware or channels, across a large number of commands without needing to define those attributes on each individual command. Shared attributes are specified in an array format as the first parameter to the `$botman->group` method.

### Drivers
A common use-case for command groups is restricting the commands to a specific messaging service using the `driver` parameter in the group array:

```php
$botman->group(['driver' => SlackDriver::class], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Only listens on Slack
    });
});
```

### Middleware
You may also group your commands, to send them through custom middleware classes. These classes can either listen for different parts of the message or extend your message by sending it to a third party service. The most common use-case would be the use of a Natural Language Processor like wit.ai or api.ai.

```php
$botman->group(['middleware' => new MyCustomMiddleware()], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Will be sent through the custom middleware object.
    });
});
```

### Channels
Command groups may also be used to restrict the commands to specific "channels", meaning they get restricted to the user sending the message to your bot. You can access the Channel-IDs in your commands using `$bot->getMessage()->getChannel();`.

```php
$botman->group(['channel' => '1234567890'], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Only listens when user/channel '1234567890' is sending the message.
    });
});
```

<a id="driver-specifics"></a>
## Driver Specifics

BotMan can listen to many different [messaging drivers](#connect-with-your-messaging-service) and therefore it might be required for you, to respond differently depending on which
driver was used to respond to your message.

Each messaging driver in BotMan has a `getName()` method, that returns a human readable name of the driver.
 
You can access the driver object using `$botman->getDriver()`.
To match against the driver name, you can use each driver's `NAME` constant or use the table below.

The available driver names are:

| Driver | Name | Note
|--- |---|---
| `BotFrameworkDriver` | BotFramework
| `FacebookDriver` | Facebook
| `FacebookPostbackDriver` | Facebook | Allows you to listen for custom Facebook Postback payloads.
| `HipChatDriver` | HipChat
| `NexmoDriver` | Nexmo
| `SlackDriver` | Slack
| `TelegramDriver` | Telegram
| `WeChatDriver` | WeChat
