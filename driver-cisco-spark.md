# Cisco Spark

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)
- [Register Your Webhook](#register-webhook)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Cisco Spark Driver.

```sh
composer require botman/driver-cisco-spark
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\CiscoSpark\CiscoSparkDriver::class);

// Create BotMan instance
BotManFactory::create($config);
```

Or if you use BotMan Studio:

```sh
php artisan botman:install-driver cisco-spark
```

This driver requires a valid and secure URL in order to set up webhooks and receive events and information from the chat users. This means your application should be accessible through an HTTPS URL.

> {callout-info} [ngrok](https://ngrok.com/) is a great tool to create such a public HTTPS URL for your local application. If you use Laravel Valet, you can create it with "valet share" as well.


To connect BotMan with your Cisco Spark Bot, you first need to follow the [official guide](https://developer.ciscospark.com/bots.html) to create your Cisco Spark Bot and an access token.

Once you have obtained the access token and secret, place it in your BotMan configuration. If you use BotMan Studio, you can find the configuration file located under `config/botman/cisco-spark.php`.

If you dont use BotMan Studio, add these line to $config array that you pass when you create the object from BotManFactory.

```php
'cisco-spark' => [
    'token' => 'YOUR-CISCO-SPARK-TOKEN-HERE',
    'secret' => 'YOUR-CISCO-SPARK-SECRET-HERE',
]
```


<a id="supported-features"></a>
## Supported Features
This is a list of features that the driver supports.
If a driver does not support a specific action, it is in most cases a limitation from the messaging service - not BotMan.

<table class="table">
<thead>
    <tr>
        <th>Feature</th>
        <th>Supported?</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>Question-Buttons</td>
        <td>❌</td>
    </tr>
    <tr>
        <td>Image Attachment</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Video Attachment</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Audio Attachment</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Location Attachment</td>
        <td>❌</td>
    </tr>
</tbody>
</table>

<a id="register-webhook"></a>
## Register Your Webhook

To let your Cisco Spark Bot know, how it can communicate with your BotMan bot, you have to register the URL where BotMan is running at,
with Cisco Spark.

You can do this by sending a `POST` request to this URL:

```url
curl -X POST -H "Accept: application/json" -H "Authorization: Bearer --YOUR-CISCO-SPARK-TOKEN--" -H "Content-Type: application/json" -d '{
    "name": "BotMan Webhook",
    "targetUrl": "--YOUR-URL--",
    "resource": "all",
    "event": "all"
}' "https://api.ciscospark.com/v1/webhooks"
```

Replace `--YOUR-CISCO-SPARK-TOKEN--` with your access token. Replace `--YOUR-URL--` with the URL that points to your BotMan logic / controller.
If you use [BotMan Studio](/__version__/botman-studio) it will be:
`https://yourapp.domain/botman`.