# Storing Information

- [Introduction](#introduction)
- [User Storage](#user-storage)
- [Channel Storage](#channel-storage)
- [Driver Storage](#driver-storage)
- [Write To Storage](#write-storage)
- [Read From Storage](#read-storage)
- [Delete Items In Storage](#delete-storage)

<a id="introduction"></a>
## Introduction
BotMan has a builtin storage system, which you can use to store user, "channel" or driver specific information without having the need for conversations.
By default, BotMan will use a simple JSON file storage to keep the data in the filesystem.

<a id="user-storage"></a>
## User Storage

The user storage is - as the name suggests - scoped to users. You can place user related information in there and also easily access stored elements for the current chatbot user, that you are interacting with:

```php
$storage = $botman->userStorage();
```

<a id="channel-storage"></a>
## Channel Storage

The channel storage can be used to save and retrieve "channel" related information. This is especially useful in combination with messaging services that support the concept of "channels" - such as Slack channels or Telegram group chts.

```php
$storage = $botman->channelStorage();
```

<a id="driver-storage"></a>
## Driver Storage

The driver storage is even more generic than the channel storage, as it is only restricted to the messaging service that is currently used. You can use this storage to store messaging service related information.

```php
$storage = $botman->driverStorage();
```

<a id="write-storage"></a>
## Write To Storage
You can write simple elements to the storage by using the `save` method on your storage instance.

```php
$bot->userStorage()->save([
    'name' => $name
]);
```

<a id="read-storage"></a>
## Read From Storage
You can read elements from the storage in multiple ways.

The `all` method will return all entries on the storage.

```php
$bot->userStorage()->all();
```

If you only want to select one specific entry / ID, you can use the `find` method:

```php
$bot->userStorage()->find($id);
```

Once you have received an item from the storage, call the `get` method to retrieve specific keys:

```php
$userinformation = $bot->userStorage()->find($id);
$element = $userinformation->get('my-custom-stored-element');
```

<a id="delete-storage"></a>
## Delete Items In Storage
To delete existing entries from the storage, use the `delete` method on the storage instance. This will delete
every information associated with the selected storage.

```php
$bot->userStorage()->delete();
```
