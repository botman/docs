# Nexmo

To connect BotMan with Nexmo, you first need to create a Nexmo account [here](https://dashboard.nexmo.com/sign-up) and [buy a phone number](https://dashboard.nexmo.com/buy-numbers), which is capable of sending SMS.

Go to the Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings) and copy your API key and API secret.

```php
'botman' => [
    'nexmo' => [
    	'key' => 'YOUR-NEXMO-APP-KEY',
    	'nexmo_secret' => 'YOUR-NEXMO-APP-SECRET',
    ]
],
```

## Register your Webhook

To let Nexmo send your bot notifications when incoming SMS arrive at your numbers, you have to register the URL where BotMan is running at,
with Nexmo.

You can do this by visiting your Nexmo dashboard at [https://dashboard.nexmo.com/settings](https://dashboard.nexmo.com/settings).

There you will find an input field called `Callback URL for Inbound Message` - place the URL that points to your BotMan logic / controller in this field.
