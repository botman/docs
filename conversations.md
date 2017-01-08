# Conversations

- [Start a Conversation](#start)
- [Control Conversation Flow](#control-conversation-flow)

<a id="start"></a>
## Start a Conversation

#### $bot->startConversation()

| Argument | Description
|---  |---
| conversation  | A `Conversation` object

`startConversation()` is a function that creates conversation in response to an incoming message. You can control where the bot should start the conversation by calling `startConversation` in the `hears()` method of your bot.

Simple conversation example:

```php
$botman->hears('start conversation', function (BotMan $bot) {
    $bot->startConversation(new PizzaConversation);
});
```

When starting a new conversation using the `startConversation()` method, you need to pass the method the conversation that you want to start gathering information with.
Each conversation object needs to extend from the BotMan `Conversation` object and must implement a simple `run()` method.

This is the very first method that gets executed when the conversation starts.

Example conversation object:

```php
class PizzaConversation extends Conversation
{
    protected $size;

    public function askSize()
    {
        $this->ask('What pizza size do you want?', function(Answer $answer) {
            // Save size for next question
            $this->size = $answer->getText();

            $this->say('Got it. Your pizza will be '.$answer->getText());
        });
    }

    public function run()
    {
        // This will be called immediately
        $this->askSize();
    }
}
``` 

<a id="control-conversation-flow"></a>
## Control Conversation Flow

#### $conversation->say()
| Argument | Description
|---  |---
| message   | String or `Question` object

Call $conversation->say() several times in a row to queue messages inside the conversation. Only one message will be sent at a time, in the order in which they are queued.

#### $conversation->ask()
| Argument | Description
|---  |---
| message   | String or `Question` object
| callback _or_ array of callbacks   | callback function in the form function($answer), or array of arrays in the form ``[ 'pattern' => regular_expression, 'callback' => function($answer) { ... } ]``

When passed a callback function, $conversation->ask will execute the callback function for any response.
This allows the bot to respond to open-ended questions, collect the responses, and handle them in whatever
manner it needs to.

When passed an array, the bot will look first for a matching pattern, and execute only the callback whose
pattern is matched. This allows the bot to present multiple choice options, or to proceed
only when a valid response has been received. 
The patterns can have the same placeholders as the `$bot->reply()` method has. All matching parameters will be passed to the callback function.

Callback functions passed to `ask()` receive (at least) two parameters - the first is an `Answer` object containing
the user's response to the question. 
If the conversation continues because of a matching pattern, all matching pattern parameters will be passed to the callback function too.
The last parameter is always a reference to the conversation itself.

##### Using $conversation->ask with a callback:

```php
// ...inside the conversation object...
public function askMood()
{
    $this->ask('How are you?', function (Answer $response) {
        $this->say('Cool - you said ' . $response->getText());
    });
}
```

##### Using $conversation->ask with an array of callbacks:


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
#### Using $conversation->ask with a Question object

Instead of passing a string to the `ask()` method, it is also possible to create a `Question` object.
The Question objects make use of the interactive messages from Facebook, Telegram and Slack to present the user buttons to interact with.

When passing question objects to the `ask()` method, the returned `Answer` object has a method called `isInteractiveMessageReply` to detect, if 
the user interacted with the message and clicked on a button.

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
