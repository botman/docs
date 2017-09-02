# Web

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Web Driver.

```sh
composer require botman/driver-web
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver web
```

This driver can be used as a starting point to add BotMan to your website or API. The driver itself receives the incoming requests and responds with a JSON representing the message result.

### Example Response
This is an example response from BotMan. It contains a `message` array that holds all messages that BotMan replies.

```json
{
    "status":200,
    "messages":[
        {
            "type":"text",
            "text":"I have no idea what you are talking about!",
            "attachment":{
                "type":"image",
                "url":"https:\/\/botman.io\/img\/logo.png",
                "title":null
            }
        }
    ]
}
```
The driver configuration allows you to define which request parameters need to be present in order for BotMan to detect the incoming request as a request for the "web" driver. By default, all HTTP requests to your BotMan controller need to contain a `driver` attribute with the value `web`.
Pass the driver configuration to the `BotManFactory` upon initialization. If you use BotMan Studio, you can find the configuration file located under `config/botman/web.php`.

```php
[
    'web' => [
    	'matchingData' => [
            'driver' => 'web',
        ],
    ]
]
```

Last you need to connect the test bot with your WeChat user account. Use the `Test account QR Code` from your sandbox page to add the bot to your contacts. And that's it - you can now use BotMan with WeChat to create interactive bots!


<a id="supported-features"></a>
## Supported Features
Since this is a "naked" web-driver implementation, it is up to you to implement the features that you need.