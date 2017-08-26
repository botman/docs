# User Information

- [Introduction](#introduction)
- [Retrieving User Information](#retrieving-user-information)

## Introduction



<a id="retrieving-user-information"></a>

## Retrieving User Information
BotMan has a simple way to let you retrieve user relevant information.

```php
$botman->hears('Hello', function($bot) {
	$user = $bot->getUser();
	$bot->reply('Hello '.$user->getFirstName().' '.$user->getLastName());
	$bot->reply('Your username is: '.$user->getUsername());
	$bot->reply('Your ID is: '.$user->getId());
});
```

> {callout-info} Not every messaging service allows BotMan to retrieve all available user information. Nexmo for example does not have any detailed user information, since it is just an SMS messaging service, that has no additional user information stored in their system.