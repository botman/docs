# Conversations

- [Start A Conversation](#starting-a-conversation)
- [Asking Questions](#asking-questions)
- [Asking For Images, Videos, Audio Or Location](#asking-for-data)
- [Structured Question With Patterns](#structured-question)
- [Originating Conversations](#originating-conversations)
- [Skipping Conversations](#skipping-conversations)
- [Caching Conversations](#caching-conversations)

When it comes to chat bots, you probably don't want to simply react to single keywords, but instead, you might need to gather information from the user, using a conversation.
Let's say, that you want your chat bot to provide an elegant user onboarding experience for your application users. In the onboarding process we are going to ask the user for their firstname and email address - that's a perfect fit for conversations!

> {callout-info} If you are **not** using [BotMan Studio](/__version__/botman-studio), you must configure a persistent [Cache Driver](/__version__/cache-drivers) to use BotMan's Conversation, as BotMan will keep the conversation state cached.

<a id="starting-a-conversation"></a>
## Starting A Conversation

You can start a conversation with your users using the `startConversation` method inside a keyword callback:

```php
$botman->hears('Hello', function($bot) {
    $bot->startConversation(new OnboardingConversation);
});
// Listen
$botman->listen();
```

Let's take a look at the `OnboardingConversation` class:

```php
class OnboardingConversation extends Conversation
{
    protected $firstname;

    protected $email;

    public function askFirstname()
    {
        $this->ask('Hello! What is your firstname?', function(Answer $answer) {
            // Save result
            $this->firstname = $answer->getText();

            $this->say('Nice to meet you '.$this->firstname);
            $this->askEmail();
        });
    }

    public function askEmail()
    {
        $this->ask('One more thing - what is your email?', function(Answer $answer) {
            // Save result
            $this->email = $answer->getText();

            $this->say('Great - that is all we need, '.$this->firstname);
        });
    }

    public function run()
    {
        // This will be called immediately
        $this->askFirstname();
    }
}
```

All conversations that your bot might use need to extend the abstract Conversation class and implement a `run` method.

This is the starting point of your conversation and get's executed immediately.

As you can see in the onboarding conversation, we have two questions that get asked one after another. Just like a regular conversation, the bot first asks for the firstname and saves the value in the conversation object itself.

After it retrieves an answer from the user, the callback gets executed and the bot asks the next question, which retrieves the user's email address.

<a id="asking-questions"></a>
## Asking Questions

BotMan gives you two ways to ask your users questions. The straight forward aproach is simply using a string as a question:

```php
// ...inside the conversation object...
public function askMood()
{
    $this->ask('How are you?', function (Answer $response) {
        $this->say('Cool - you said ' . $response->getText());
    });
}
```

Instead of passing a string to the `ask()` method, it is also possible to create a question object.
The question objects make use of the interactive messages from supported messaging services to present the user buttons to interact with.
<br><br>
When passing question objects to the `ask()` method, the returned `Answer` object has a method called `isInteractiveMessageReply` to detect, if
the user interacted with the message and clicked on a button or simply entered text.
<br><br>
Creating a simple Question object:

```php
// ...inside the conversation object...
public function askForDatabase()
{
    $question = Question::create('Do you need a database?')
        ->fallback('Unable to create a new database')
        ->callbackId('create_database')
        ->addButtons([
            Button::create('Of course')->value('yes'),
            Button::create('Hell no!')->value('no'),
        ]);

    $this->ask($question, function (Answer $answer) {
        // Detect if button was clicked:
        if ($answer->isInteractiveMessageReply()) {
            $selectedValue = $answer->getValue(); // will be either 'yes' or 'no'
            $selectedText = $answer->getText(); // will be either 'Of course' or 'Hell no!'
        }
    });
}
```

<a id="asking-for-data"></a>
## Asking For Images, Videos, Audio or Location

With BotMan, you can easily let your bot [receive images, videos, audio files or locations](/__version__/receiving-additional-content).
The same approach can be applied to your conversation. This is especially useful if you only want your bot users to provide you with specific attachment types.

You might use the `askForImages` method to ask your bot users a question and only accept one or more images as a valid answer:

```php
// ...inside the conversation object...
public function askPhoto()
{
    $this->askForImages('Please upload an image.', function ($images) {
        // $images contains an array with all images.
    });
}
```

The callback will receive an array containing all images the user has sent to the bot. By default, if the user does not provide a valid image, the question will just be repeated. But you can alter it, by specifying a third parameter. This is the callback that will be executed, once the provided message is not a valid image.

```php
// ...inside the conversation object...
public function askPhoto()
{
    $this->askForImages('Please upload an image.', function ($images) {
        //
    }, function(Answer $answer) {
        // This method is called when no valid image was provided.
    });
}
```

Similar to the `askForImages` method, you might also ask for videos, audio files or even your chatbot user's locations.

```php
// ...inside the conversation object...
public function askVideos()
{
    $this->askForVideos('Please upload a video.', function ($videos) {
        // $videos is an array containing all uploaded videos.
    });
}

public function askAudio()
{
    $this->askForAudio('Please upload an audio file.', function ($audio) {
        // $audio is an array containing all uploaded audio files.
    });
}

public function askLocation()
{
    $this->askForLocation('Please tell me your location.', function (Location $location) {
        // $location is a Location object with the latitude / longitude.
    });
}
```

<a id="structured-question"></a>

## Structured Question With Patterns

You might also want to ask your user questions, where you already know the answer should be in a fixed set of possible options.
For example a simple yes or no question.

Instead of passing a closure to the `ask` method, you can simply pass it an array.
BotMan will then look first for a matching pattern, and execute only the callback whose pattern is matched.

This allows the bot to present multiple choice options, or to proceed only when a valid response has been received. The patterns can have the same placeholders as the `$bot->reply()` method has. All matching parameters will be passed to the callback function.

```php
// ...inside the conversation object...
public function askNextStep()
{
    $this->ask('Shall we proceed? Say YES or NO', [
        [
            'pattern' => 'yes|yep',
            'callback' => function () {
                $this->say('Okay - we\'ll keep going');
            }
        ],
        [
            'pattern' => 'nah|no|nope',
            'callback' => function () {
                $this->say('PANIC!! Stop the engines NOW!');
            }
        ]
    ]);
}

```

<a id="originating-conversations"></a>
## Originating Conversations

Just like it is possible with BotMan to [originate messages](/__version__/sending#originating-messages), it is also possible to start complete conversations on the bot's behalf. You might, for example, start a new conversation that gets triggered through your cronjob.

To do so, just use the `startConversation` method, as you would normally do, but pass a user ID and the driver name as additional arguments:

```php
$botman->startConversation(new PizzaConversation(), 'my-recipient-user-id', TelegramDriver::class);
```

<a id="skipping-conversations"></a>
## Skip or Stop Conversations

While being in a conversation it is sometimes useful to stop it for a certain reason. In order to make this possible there a two methods available in every conversation class: `skipConversation()` and `stopConversation()`. Inside those methods you can return true or false to skip or stop a conversation. Skip will stop the conversation just for this one request. Afterwards it will go on as if nothing happened. The stop method will delete the conversation, so there is no turning back.

The example below will stop the conversation if the message says `stop` and skip it if it says `pause`.

```php
class OnboardingConversation extends Conversation
{
    protected $firstname;

    protected $email;

    public function stopConversation(Message $message)
	{
		if ($message->getMessage() == 'stop') {
			return true;
		}

		return false;
	}

	public function skipConversation(Message $message)
	{
		if ($message->getMessage() == 'pause') {
			return true;
		}

		return false;
	}

    // ...

}
```

There are several scenarios where this could be handy. For example in Facebook there is a menu in the chat. When the user clicks a menu button you want to leave a current conversation in order to show the new information. But it also possible to use this functionality on a "global" level.

```php
$botman->hears('stop', function(BotMan $bot) {
	$bot->reply('stopped');
})->stopsConversation();
```

Here we are stopping the conversation through a `hears` method, where we are listening for the message `stop`. This comes handy when we want to stop a conversation, not matter which one it is. This also works for skipping.

```php
$botman->hears('pause', function(BotMan $bot) {
	$bot->reply('stopped');
})->skipsConversation();
```

<a id="caching-conversations"></a>
## Caching Conversations

Conversations get persisted in the cache. This means that the conversation class will be serialized in order to maintain the conversation state. Keep that in mind when developing for BotMan. The default cache duration is set to 30 minutes. If you need to increase or decrease this value, you can set a `conversation_cache_time` key on your BotMan configuration.

```php
'botman' => [
	'conversation_cache_time' => 30
],
```
