# Events

- [Introduction](#introduction)
- [Listening To Driver Events](#listening)
- [Available Events](#available-events)

<a id="introduction"></a>
## Introduction
BotMan gives you the ability to listen to driver specific events, such as when a person joins your Slack channel or a Facebook message gets read. You can then perform various tasks when these events happen and reply to your chatbot users.

<a id="listening"></a>
## Listening To Driver Events

You may listen to driver events using a simple and easy API. The BotMan object provides an `on` method that takes an event name and a closure/callable to execute once the event occurs.
<br><br>
All callbacks will receive the event payload and the BotMan instance as parameters.

```php
$botman->on('event', function($payload, $bot) {
	
});
```

<a id="available-events"></a>
## Available Events
Please refer to each messaging service documentation to get a list of available events.