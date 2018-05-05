# Upgrade Guide

- [Upgrading To 2.0 From 1.5](#upgrade-2.0)

<a id="upgrade-2.0"></a>

## Upgrading To 2.0 From 1.5

The BotMan 2.0 release is a new major version release. While the first version of BotMan was provided under the personal vendor namespace `mpociot`, the project is now moved into a separate [GitHub organization](https://github.com/botman/). This results in a change of all BotMan classes, since they now live in a different namespace than in version 1.5.

### Changing Dependencies

Change your `mpociot/botman` dependency to `botman/botman` with version `2.0.*` in your `composer.json` file. 

### Removed Drivers
In favor of the new [driver events](/__version__/driver-events), we have removed the following drivers:

`- FacebookOptinDriver`<br>
`- FacebookReferralDriver`
<br><br>
To react to Optin or Referral events, use the new driver event syntax:

```php
// FacebookReferralDriver
$botman->on('messaging_referrals', function($payload, $bot) {
	
});
// FacebookOptinDriver
$botman->on('messaging_optins', function($payload, $bot) {
	
});
```

### Installing Drivers
BotMan 2.0 does no longer have the messaging service specific drivers included in the core repository. This allows easier maintenance and bugfixing of the BotMan core, while also speeding up the development of individual messaging service drivers.

You can find a complete list of the available drivers in the [BotMan GitHub organization](https://github.com/botman?utf8=%E2%9C%93&q=driver-&type=&language=).

Install all needed drivers via composer as a separate package. Please note that the driver configuration structure also changed. Take a look at the individual driver section in the documentation to verify the new configuration array structure for your driver.

Make sure to load all manually installed drivers via the `DriverManager::loadDriver` method in order to make it accessible to BotMan.

### Command `channel` Group
The `channel` command group has been renamed to `recipient`.

### Sending Messages And Attachments
The `Message` class was renamed to `BotMan\BotMan\Messages\Outgoing\OutgoingMessage`. Instead of providing `image`, `video`, etc. methods for attachments, those attachments are now separated in different classes and can be sent using the `withAttachment` method.
<br><br>
Take a look at the [Sending Messages](/__version__/sending#attachments) documentation for code examples.

### Storage `get` Method
The Storage classes now have a `find` method, that replaces the `get` method. This allows easier access of default entry keys.
Replace all your `get` occurences with `find`.

### Middleware
The middleware system of BotMan 2.0 is completely rewritten and provides a lot more flexibility than the 1.x version did.

### API.ai
If you use API.ai in combination with your BotMan commands, initialize the middleware and pass it to the new global `received` middlewares on BotMan.
You can then use the middleware inside your `hears` methods as usual.

```php
$middleware = ApiAi::create('your-api-ai-token')->listenForAction();

// Apply global "received" middleware
$botman->middleware->received($middleware);
```
