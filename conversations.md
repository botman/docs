# Conversations

- [Start a Conversation](#start-a-conversation)
- [Asking Questions](#asking-questions)
- [Structured Question with Patterns](#structured-question)

When it comes to chat bots, you probably don't want to simply react to single keywords, but instead, you might need to gather information from the user, using a conversation. 
Let's say, that you want your chat bot to provide an elegant user onboarding experience for your application users. In the onboarding process we are going to ask the user for their firstname and email address - that's a perfect fit for conversations!

<a id="starting-a-conversation"></a>
## Starting a Conversation
You can start a conversation with your users using the `startConversation` method inside a keyword callback:

```php
$botman->hears('Hello', function($bot) {
    $bot->startConversation(new OnboardingConversation);
});
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

> {callout-info} Conversations get persisted in the cache. This means that the conversation class will be serialized in order to maintain the conversation state. Keep that in mind when developing for BotMan. The default cache duration is set to 30 minutes. If you need to increase or decrease this value, you can set a `conversation_cache_time` key on your BotMan configuration.

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

When passing question objects to the `ask()` method, the returned `Answer` object has a method called `isInteractiveMessageReply` to detect, if 
the user interacted with the message and clicked on a button or simply entered text.

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

<a id="patterns"></a>

## Structured Question with Patterns

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
