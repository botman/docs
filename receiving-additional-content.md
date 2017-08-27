# Hearing Attachments

- [Introduction](#introduction)
- [Images](#images)
- [Videos](#videos)
- [Audio](#audio)
- [Locations](#locations)

<a id="introduction"></a>
## Introduction

BotMan does not only allow you to listen for incoming textual messages, but also gives you the ability to listen for incoming images, videos, audio messages and locations.

<a id="images"></a>
## Listen for images

Making your bot listen to incoming images is easy. Just use the `receivesImages` method on the BotMan instance. All received images will be passed to the callback as a second argument.

The images returned will be an array containing an URL to access the uploaded image file.

```php
$bot->receivesImages(function($bot, $images) {
    //
});
```

<a id="videos"></a>
## Listen for videos

Just like images, you can use the `receivesVideos` method on the BotMan instance to listen for incoming video file uploads. All received videos will be passed to the callback as a second argument.

The videos returned will be an array containing an URL to access the uploaded video file.

```php
$bot->receivesVideos(function($bot, $videos) {
    //
});
```

<a id="audio"></a>
## Listen for audio

Just like images, you can use the `receivesAudio` method on the BotMan instance to listen for incoming audio file uploads. All received audio files will be passed to the callback as a second argument.

The audio files returned will be an array containing an URL to access the uploaded files.

```php
$bot->receivesAudio(function($bot, $audio) {
    //
});
```

<a id="locations"></a>
## Listen for locations

Some messaging services also allow your bot users to send their GPS location to your bot. You can listen for these location calls using the `receivesLocation` method on the BotMan instance.

The method will pass a Location object to the callback method.

```php
use BotMan\BotMan\Messages\Attachments\Location;

$bot->receivesLocation(function($bot, Location $location) {
    $lat = $location->getLatitude();
    $lng = $location->getLongitude();
});
```