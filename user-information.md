# User Information

- [Introduction](#introduction)
- [Retrieving User Information](#retrieving-user-information)
- [Retrieve The User's First Name](#retrieving-user-firstname)
- [Retrieve The User's Last Name](#retrieving-user-lastname)
- [Retrieve The User-ID](#retrieving-user-id)
- [Retrieve The Username](retrieving-username)
- [Retrieve The Raw User Information](#retrieving-raw-user-information)
- [Caching User Information](#caching-user-information)

## Introduction
BotMan makes it simple to access your chatbot user information. Which user fields are available purely depend on the messaging service you use. Some services offer more information than others do. Yet you can access them using the same API with BotMan.

<a id="retrieving-user-information"></a>

## Retrieving User Information
BotMan has a simple way to let you retrieve user relevant information. Once you listened to a command and are in either a conversation or a message callback, you can use the `getUser` method on the BotMan instance.

<a id="retrieving-user-firstname"></a>
### Retrieve The User's First Name
You can access the user's first name using:

```php
// Access user
$user = $bot->getUser();
// Access first name
$firstname = $user->getFirstName();
```

<a id="retrieving-user-lastname"></a>
### Retrieve The User's Last Name
You can access the user's last name using:

```php
// Access user
$user = $bot->getUser();
// Access last name
$lastname = $user->getLastName();
```

<a id="retrieving-user-id"></a>
### Retrieve The User-ID
You can access the user ID using:

```php
// Access user
$user = $bot->getUser();
// Access ID
$id = $user->getId();
```

<a id="retrieving-username"></a>
### Retrieve The Username
You can access the user ID using:

```php
// Access user
$user = $bot->getUser();
// Access ID
$id = $user->getUsername();
```

<a id="retrieving-raw-user-information"></a>
### Retrieve The Raw User Information
You can access also access the unprocessed raw messaging service user information that BotMan receives using:

```php
// Access user
$user = $bot->getUser();
// Access ID
$info = $user->getInfo();
```

<a id="caching-user-information"></a>
### Caching User Information

BotMan will cache user information for a duration of 30 minutes by default. This is especially useful for drivers like
Facebook where it takes an additional request to retrieve this information. In the BotMan config you are able to
change this duration with the `user_cache_time`. You can also set it to zero in order to prevent BotMan from caching the user information at all.

```php
'botman' => [
	'user_cache_time' => 30
],
```