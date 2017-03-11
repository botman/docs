# Storing Information

BotMan has a builtin storage system, which you can use to store user, team or driver specific information without having the need for conversations.
By default, BotMan will use a simple JSON file storage to keep the data in the filesystem.

To access the different storage types, BotMan provides these functions:

`userStorage()` - Will give you a storage that automatically uses the current message user as the default key.

`channelStorage()` - Will give you a storage that automatically uses the current message channel as the default key.

`driverStorage()` - Will give you a storage that automatically uses the current driver name as the default key.

### Example usage

```php
$botman->hears("forget me", function (BotMan $bot) {
    // Delete all stored information. 
    $bot->userStorage()->delete();
});

$botman->hears("call me {name}", function (BotMan $bot, $name) {
    // Store information for the currently logged in user.
    // You can also pass a user-id / key as a second parameter.
    $bot->userStorage()->save([
        'name' => $name
    ]);

    $bot->reply('I will call you '.$name);
});

$botman->hears("who am I", function (BotMan $bot) {
    // Retrieve information for the currently logged in user.
    // You can also pass a user-id / key as a second parameter.
    $user = $bot->userStorage()->get();

    if ($user->has('name')) {
        $bot->reply('You are '.$user->get('name'));
    } else {
        $bot->reply('I do not know you yet.');
    }
});
```
