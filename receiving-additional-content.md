# Hearing Attachments

- [Introduction](#introduction)
- [Images](#images)
- [Videos](#videos)
- [Audio](#audio)
- [Files](#files)
- [Locations](#locations)

<a id="introduction"></a>
## Introduction

BotMan does not only allow you to listen for incoming textual messages, but also gives you the ability to listen for incoming images, videos, audio messages and locations.
You need to manually load corresponding driver, for example:

```php
DriverManager::loadDriver(\BotMan\Drivers\Telegram\TelegramPhotoDriver::class);
```

<a id="images"></a>
## Listen for images

Making your bot listen to incoming images is easy. Just use the `receivesImages` method on the BotMan instance. All received images will be passed to the callback as a second argument.

The images returned will be an array of `Image` objects.

```php
use BotMan\BotMan\Messages\Attachments\Image;

$bot->receivesImages(function($bot, $images) {

    foreach ($images as $image) {

        $url = $image->getUrl(); // The direct url
        $title = $image->getTitle(); // The title, if available
        $payload = $image->getPayload(); // The original payload
    }
});
```

<a id="videos"></a>
## Listen for videos

Just like images, you can use the `receivesVideos` method on the BotMan instance to listen for incoming video file uploads. All received videos will be passed to the callback as a second argument.

The videos returned will be an array of `Video` objects.

```php
use BotMan\BotMan\Messages\Attachments\Video;

$bot->receivesVideos(function($bot, $videos) {

    foreach ($videos as $video) {

        $url = $video->getUrl(); // The direct url
        $payload = $video->getPayload(); // The original payload
    }
});
```

<a id="audio"></a>
## Listen for audio

Just like images, you can use the `receivesAudio` method on the BotMan instance to listen for incoming audio file uploads. All received audio files will be passed to the callback as a second argument.

The audio files returned will be an array of `Audio` objects.

```php
use BotMan\BotMan\Messages\Attachments\Audio;

$bot->receivesAudio(function($bot, $audios) {

    foreach ($audios as $audio) {

        $url = $audio->getUrl(); // The direct url
        $payload = $audio->getPayload(); // The original payload
    }
});
```

<a id="files"></a>
## Listen for files

Just like images, you can use the `receivesFiles` method on the BotMan instance to listen for incoming file uploads. All received files will be passed to the callback as a second argument.

The files returned will be an array of `File` objects.

```php
use BotMan\BotMan\Messages\Attachments\File;

$bot->receivesFiles(function($bot, $files) {

    foreach ($files as $file) {

        $url = $file->getUrl(); // The direct url
        $payload = $file->getPayload(); // The original payload
    }
});
```

<a id="locations"></a>
## Listen for locations

Some messaging services also allow your bot users to send their GPS location to your bot. You can listen for these location calls using the `receivesLocation` method on the BotMan instance.

The method will pass a `Location` object to the callback method.

```php
use BotMan\BotMan\Messages\Attachments\Location;

$bot->receivesLocation(function($bot, Location $location) {
    $lat = $location->getLatitude();
    $lng = $location->getLongitude();
});
```

## Supported Drivers

Not all drivers support receiving attachments, or because of the lack of reference API, or because it has not yet been implemented in the driver itself.

| Can (Receive/Send) | Images | Videos | Audio | Files | Locations |
|--------------------|--------|--------|-------|-------|-----------|
| Telegram           |   ✔/✔  |   ✔/✔  |  ✔/✔  |  ✔/✔  |    ✔/✔    |
| Facebook           |   ✔/✔  |   ✔/✔  |  ✔/✔  |  ✔/✔  |    ✔/❌    |
| Slack              |   ✔/✔  |   ✔/❌  |  ✔/❌  |  ✔/✔  |    ❌/❌    |
| Kik                |   ✔/✔  |   ✔/✔  |  ❌/❌  |  ❌/❌  |    ❌/❌    |
| WeChat             |   ✔/✔  |   ✔/❌  |  ✔/❌  |  ❌/❌  |    ✔/❌    |
| HipChat            |   ❌/❌  |   ❌/❌  |  ❌/❌  |  ❌/❌  |    ❌/❌    |
| Nexmo              |   ❌/❌  |   ❌/❌  |  ❌/❌  |  ❌/❌  |    ❌/❌    |
