# Testing

- [Introduction](#introduction)
- [Testing multiple replies](#testing-multiple-replies)
- [Testing Questions](#testing-questions)
- [Testing Conversations](#testing-conversations)
- [Testing Facebook Templates](#testing-facebook-templates)

## Introduction

Like Laravel, BotMan is build with testing in mind. Every part of it is well tested and this is the only way to keep such a big project running properly.

But also your bots should be built with tests and this is why BotMan Studio will help you with that. Testing bots and conversations is very different from your other projects. This is why BotMan Studio provides lots of helper methods for you. This is the basic test example you will find in your `ExampleTest.php` file.

```php
	public function testBasicTest()
	{
		$this->bot
			->receives('Hi')
			->assertReply('Hello!');
	}
```
<a id="testing-multiple-replies"></a>
### Testing multiple replies
If your bot replies with more than one message to `Hi` ,you can you use the `assertReplies()` helper.

```php
public function testBasicTest()
{
	$this->bot
		->receives('Hi')
		->assertReplies([
			'Hello!',
			'Nice to meet you',
		]);
}
```

<a id="testing-questions"></a>
## Testing Questions
To check if the bot replies to a message with a question object, you can use the `assertReply` method as well. This way you can make sure the question, settings and buttons are the same as in your application.
```php
public function testStartConversation()
{
	 $question = Question::create("Huh - you woke me up. What do you need?")
				->fallback('Unable to ask question')
				->callbackId('ask_reason')
				->addButtons([
					Button::create('Tell a joke')->value('joke'),
					Button::create('Give me a fancy quote')->value('quote'),
				]);
	
	$this->bot
		->receives('Start Conversation')
		->assertReply($question);
}
```

<a id="testing-conversations"></a>
## Testing Conversations
When you want to test a longer conversation you can chain the given methods. Just add another `receives()` and an `assertion` afterwards.

```php
public function testStartConversation()
{
	$this->bot
		->receives('Hi')
		->assertReplies([
			'Hello!',
			'Nice to meet you. What is your name?',
		])->receives('BotMan')
		->assertReply('BotMan, that is a beautifule name :-)');
}
```

<a id="testing-facebook-templates"></a>
## Testing Facebook Templates

Templates, like the ones for the Facebook Messenger, can be tested like the Question objects with the `assertReply()` or `assertReplies()` methods.

```php
public function testStartJokeConversation()
{
	$jokeTypeQuestion = ButtonTemplate::create('Please select the kind of joke you wanna hear:')
				->addButtons([
					ElementButton::create('Chuck Norris')->type('payload')->payload('chucknorris'),
					ElementButton::create('Children')->type('payload')->payload('children'),
					ElementButton::create('Politics')->type('payload')->payload('politics'),
				]);
	
	$this->bot->receives('start joke conversation')
		->assertReplies([
			'Ok lets go!',
			$jokeTypeQuestion
	]);
}
```