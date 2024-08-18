# Slack

- [Installation & Setup](#installation-setup)
- [Supported Features](#supported-features)
- [Usage with a Slack App (recommended)](#slack-app)
- [Usage with the Realtime API](#realtime-api)
- [Usage with an outgoing webhook](#outgoing-webhook)
- [Sending Slack Menus](#sending-slack-menus)

<a id="installation-setup"></a>
## Installation & Setup

First you need to pull in the Slack Driver.

```sh
composer require botman/driver-slack
```

Then load the driver before creating the BotMan instance (**only when you don't use BotMan Studio**):

```php
DriverManager::loadDriver(\BotMan\Drivers\Slack\SlackDriver::class);

// Create BotMan instance
$config = [
    'slack' => [
        'token' => 'YOUR-SLACK-BOT-TOKEN'
    ]
];
BotManFactory::create($config);
```

Slack is a cloud-based set of team collaboration tools and services.

> {callout-info} If you're using [ngrok](https://ngrok.com/) be aware that Slack has a timeout of 3 seconds. When using ngrok, depending on your location and connection speed, the time it takes for Slack to get a 200 OK can be longer than 3 seconds. In such cases you'll notice that your bot will answer multiple times. If you're facing this issue a possible solution is to deploy your bot to a production server or try to change the ngrok server location.

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
        <td>✅</td>
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

<a id="slack-app"></a>
## Usage with a Slack App (recommended)

The easiest way of making use of all BotMan features in your Slack team, is to create your own Slack app. You can do this at the [Slack API website](https://api.slack.com/apps?new_app=1).

Once you have created a new Slack app, you need to configure three sections:

#### Interactive Messages
Interactive messages allow your chatbot to respond to message buttons. If you want to use this feature, just configure it and provide the URL that points to your BotMan logic / controller.

#### Event Subscriptions
Event subscription is needed, so that your BotMan application receives all incoming messages. You can either use event subscriptions or the Realtime API.
When configuring your Slack app for event subscriptions, provide the URL that points to your BotMan logic / controller in the "Request URL" field.

Then subscribe for these bot events:
`message.channels` - A message was posted to a channel
`message.im` - A message was posted in a direct message channel

#### Bots
This is the most obvious part - to communicate with BotMan in your Slack team, you need a bot user. Just give it a nice name.

After you have configured these sections, go to "Install App" and install the app in your own team.
Take note of the *Bot User OAuth Access Token* and use this token as your `slack_token` configuration parameter.

That's it.


<a id="realtime-api"></a>


<a id="outgoing-webhook"></a>
## Usage with an outgoing webhook

Add a new "Outgoing Webhook" integration to your Slack team - this URL needs to point to the controller where your BotMan bot is living in. You will find this integration when you search for `Outgoing WebHooks` in the Slack Apps Directory.

To let BotMan listen to all incoming messages, do **not** specify a trigger word, but define a channel instead.

With the Webhook implementation, there is no need to add a `slack_token` configuration. 

<a id="sending-slack-menus"></a>
## Sending Slack Menus

BotMan supports sending Slack's [interactive message menus](https://api.slack.com/docs/message-menus) through an expressive and easy to use API.
They can be used in combination with BotMan's [conversation questions](/conversations#asking-questions).

There are multiple ways to define an interactive message menu. You can simply attach a `Menu` object to your question, just as you would with regular Buttons.

You can access the selected option(s) using `$answer->getValue()` which will then return an array containing the selected option values.

### Custom drop down menu 

<center>
	<img width="366" src="/img/slack/interactive_menus.png" />
</center>

```php
// Inside your conversation
$question = Question::create('Would you like to play a game?')
    ->callbackId('game_selection')
    ->addAction(
    	Menu::create('Pick a game...')
    		->name('games_list')
    		->options([
    			[
    				'text' => 'Hearts',
    				'value' => 'hearts',
    			],
    			[
    				'text' => 'Bridge',
    				'value' => 'bridge',
    			],
    			[
    				'text' => 'Poker',
    				'value' => 'poker',
    			]
    		])
    );

$this->ask($question, function (Answer $answer) {
    $selectedOptions = $answer->getValue();
});
```

### Allow users to select from a list of team members

<center>
	<img width="366" src="/img/slack/interactive_menus_users.png" />
</center>

```php
// Inside your conversation
$question = Question::create('Who wins the lifetime supply of chocolate?')
    ->callbackId('select_users')
    ->addAction(
    	Menu::create('Who should win?')
    		->name('winners_list')
    		->chooseFromUsers()
    );

$this->ask($question, function (Answer $answer) {
    $selectedOptions = $answer->getValue();
});
```

### Let users choose one of their team's channels

<center>
	<img width="366" src="/img/slack/interactive_menus_channels.png" />
</center>

```php
// Inside your conversation
$question = Question::create('It\'s time to nominate the channel of the week')
    ->callbackId('select_channel')
    ->addAction(
    	Menu::create('Which channel changed your life this week?')
    		->name('channels_list')
    		->chooseFromChannels()
    );

$this->ask($question, function (Answer $answer) {
    $selectedOptions = $answer->getValue();
});
```

### Let users choose one of their conversations

```php
// Inside your conversation
$question = Question::create('Let\'s get a productive conversation going')
    ->callbackId('select_conversation')
    ->addAction(
    	Menu::create('Who did you talk to last?')
    		->name('conversations_list')
    		->chooseFromConversations()
    );

$this->ask($question, function (Answer $answer) {
    $selectedOptions = $answer->getValue();
});
```
