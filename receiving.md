# Hearing Messages

- [Basic Commands](#basic-commands)
- [Command Parameters](#command-parameters)
- [Matching Regular Expression](#matching-regular-expressions)
- [Command Groups](#command-groups)
    - [Drivers](#command-groups-drivers)
    - [Middleware](#command-groups-middleware)
    - [Recipients](#command-groups-recipients)
- [Fallbacks](#fallbacks)
- [Get full request payload](#payload)

<a id="basic-commands"></a>
## Basic Commands

Usually all the commands that your chatbot understands are listed in one location. You can think of it as a `routes` definition, but for your chatbot. The default location when using BotMan Studio is at `routes/botman.php`.
This way, the BotMan framework knows about all the different patterns and messages that your chatbot should listen to.

> {callout-info} If you are **not** using BotMan Studio, you must call the "$bot->listen();" method at the end of your command definition.

You can specify these commands by providing a string that your chatbot should listen for, followed by a Closure that will be executed, once the incoming message matches the string you provided.

In this example, we are listening for the incoming message "Hello" and reply "World" from our bot.

```php
$botman->hears('My First Message', function ($bot) {
    $bot->reply('Your First Response');
});
```
<a href="#" onclick="botmanChatWidget.say('My first message');return false;" class="w-full block text-right font-bold">Try it out</a>

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
For example, you may need to listen for a bot command that captures a user's name. You may do so by defining command parameters using curly braces inside of the matching string, like this:

```php
$botman->hears('call me {name}', function ($bot, $name) {
    $bot->reply('Your name is: '.$name);
});
```
<a href="#" onclick="botmanChatWidget.say('Call me Marcel');return false;" class="w-full block text-right font-bold">Try it out</a>

Of course you are not limited to only one parameter in your incoming message. You may define as many command parameters as required by your command:

```php
$botman->hears('call me {name} the {adjective}', function ($bot, $name, $adjective) {
    $bot->reply('Hello '.$name.'. You truly are '.$adjective);
});
```
<a href="#" onclick="botmanChatWidget.say('Call me Gandalf the white');return false;" class="w-full block text-right font-bold">Try it out</a>

Command parameters are always encased within `{}` braces and should consist of alphabetic characters only.

<a id="matching-regular-expressions"></a>
## Matching Regular Expressions

If command parameters do not give you enough power and flexibility for your bot commands, you can also use more complex regular expressions in your bot commands. For example, you may want your bot to only listen for digits. You may do so by defining a regular expression in your command like this:

```php
$botman->hears('I want ([0-9]+)', function ($bot, $number) {
    $bot->reply('You will get: '.$number);
});
```
<a href="#" onclick="botmanChatWidget.say('I want 3');return false;" class="w-full block text-right font-bold">Try it out</a>

Just like the command parameters, each regular expression match group will be passed to the handling Closure. So if you define to regular expression matching groups, you can access them as two different variables:

```php
$botman->hears('I want ([0-9]+) portions of (Cheese|Cake)', function ($bot, $amount, $dish) {
    $bot->reply('You will get '.$amount.' portions of '.$dish.' served shortly.');
});
```
<a href="#" onclick="botmanChatWidget.say('I want 4 portions of Cake');return false;" class="w-full block text-right font-bold">Try it out</a>

In addition to using regular expression matching groups, you can also use regular epxressions to generally match incoming requests.

Take this example, which listens for either "Hi" or "Hello" anywhere in the incoming message:

```php
$botman->hears('.*Bonjour.*', function ($bot) {
    $bot->reply('Nice to meet you!');
});
```
<a href="#" onclick="botmanChatWidget.say('Well Bonjour I\'d say');return false;" class="w-full block text-right font-bold">Try it out</a>

<a id="command-groups"></a>
## Command Groups

Command groups allow you to share command attributes, such as middleware or channels, across a large number of commands without needing to define those attributes on each individual command. Shared attributes are specified in an array format as the first parameter to the `$botman->group` method.

<a id="command-groups-drivers"></a>
### Drivers
A common use-case for command groups is restricting the commands to a specific messaging service using the `driver` parameter in the group array.
Like this, you can limit the chatbot logic for one - or multiple messenger services. In this case, the `keyword` method will only be called when the incoming message comes from Slack or Facebook:

```php
$botman->group(['driver' => [SlackDriver::class, FacebookDriver::class], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Only listens on Slack or Facebook
    });
});
```

<a id="command-groups-middleware"></a>
### Middleware
You may also group your commands, to send them through custom middleware classes. These classes can either listen for different parts of the message or extend your message by sending it to a third party service. The most common use-case would be the use of a Natural Language Processor like wit.ai or Dialogflow. To learn more about how to create your own middleware, take a look at the [middleware documentation](/__version__/middleware).

```php
$botman->group(['middleware' => new MyCustomMiddleware()], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Will be sent through the custom middleware object.
    });
});
```

<a id="command-groups-recipients"></a>
### Recipients
Command groups may also be used to restrict the commands to specific recipients. This can be especially useful, if you have messaging services that have multiple "channels". Then the "recipient" would be the channel/group that you have.

```php
$botman->group(['recipient' => '1234567890'], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Only listens when recipient '1234567890' is receiving the message.
    });
});
```

You may also use an array of recipients to restrict certain commands.

```php
$botman->group(['recipient' => ['1234567890', '2345678901', '3456789012']], function($bot) {
    $bot->hears('keyword', function($bot) {
        // Only listens when recipient '1234567890', '2345678901' or '3456789012' is receiving the message.
    });
});
```

<a id="fallbacks"></a>
## Fallbacks

BotMan allows you to create a fallback method, that gets called if none of your "hears" patterns matches. You may use this feature to give your bot users guidance on how to use your bot.

```php
$botman->fallback(function($bot) {
    $bot->reply('Sorry, I did not understand these commands. Here is a list of commands I understand: ...');
});
```
<a href="#" onclick="botmanChatWidget.say('trigger fallback');return false;" class="w-full block text-right font-bold">Try it out</a>

<a id="payload"></a>
## Get full request payload

BotMan does already a great job in providing you with data from the incoming message requests with helpers like `$bot->getUser();` or `$bot->getDriver();`. But sometimes messenger add extra data to their requests which you can't access directly like that. This is when it gets useful to grab the whole payload of the incoming request with `$bot->getMessage()->getPayload();`. This way you have full access to all the data and can decide for yourself what you need and how to process it.
