# Testing

- [Introduction](#introduction)
- [Testing Multiple Replies](#testing-multiple-replies)
- [Simulating User Messages](#simulationg-user-messages)
- [Available Assertions](#available-assertions)
- [Testing Conversations](#testing-conversations)
- [Testing Events](#testing-events)
- [Advanced Testing](#advanced-testing)

> {callout-info} The BotMan testing features are only available in combination with [BotMan Studio](/__version__/botman-studio).

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
### Testing Multiple Replies
All assertion methods can be chained to test cases when your bot responds with more than one message.

```php
public function testBasicTest()
{
	$this->bot
		->receives('Hi')
		->assertReply('Hello!')
		->assertReply('Nice to meet you');
}
```

Additionally you can you use the `assertReplies()` method if you want to test multiple replies at once.

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

<a id="simulating-user-messages"></a>
## Simulating User Messages
### Receiving Attachments

BotMan can receive attachment objects from user and there are methods that help you test those cases. You can use `receivesLocation`, `receivesImages`, `receivesVideos`, `receivesAudio` and `receivesFiles` methods to simulate that user has send valid attachments.

```php
public function testBasicTest()
{
    $this->bot
	->receivesLocation()
	->assertReply('Thanks for locations');
}
```

You can pass additional `$latitude` and `$longitude` arguments to `receivesLocation` if necessary.

```php
public function testBasicTest()
{
	$this->bot
    		->receivesLocation($latitude, $longitude)
    		->assertReply('Your latidude is ' . $latitude);
}
```

You can pass additional array of urls to `receivesImages`, `receivesVideos`, `receivesAudio` and `receivesFiles` methods if necessary.

```php
public function testBasicTest()
{
	$this->bot
    		->receivesImage(['www.example.com/dog'])
    		->assertReply('You sent me dog image');
}
```

### Receiving Interactive message
There are cases when we expect our chatbot user to respond to a question with an interactive reply - for example a button click. To simulate that, use the `receivesInteractiveMessage` method. It accepts the button value as an argument.

```php
public function testBasicTest()
{
	$this->bot
    		->receivesInteractiveMessage('dog')
    		->assertReply('You chose dog!');
}
```

<a id="available-assertions"></a>
## Available Assertions
BotMan comes with handful of useful methods to test you chatbots.

### assertReply
The `assertReply` method asserts that the chatbot replies with the given message.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReply('Hello');
}
```

### assertReplies
The `assertReplies` method asserts that the chatbot replies with all messages in the given array.

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

### assertReplyIsNot
The `assertReplyIsNot` method asserts that the chatbot does not reply with the given message.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReplyIsNot('Good bye');
}
```

### assertReplyIn
The `assertReplyIn` method asserts that the chatbot replies with one message from the given array. This is helpful if your bot uses random answers.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReplyIn([
    		    'Hi',
    		    'Hello',
    		    'Bonjourno'
    		]);
}
```

### assertReplyNotIn
The `assertReplyNotIn` method asserts that the chatbot does not reply with any of the given messages.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReplyNotIn([
    		    'Bye',
    		    'Good bye',
    		    'Arrivederci'
    		]);
}
```

### assertReplyNothing
The `assertReplyNothing` method asserts that the chatbot does not reply with any message at all. Useful when chained with other assertion methods to check that the chatbot stopped responding.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReply('Hello')
    		->assertReplyNothing();
}
```

### assertQuestion
The `assertQuestion` method asserts that the chatbot replies with a Question object. If you call this method without an argument it just checks for a question occurance. Otherwise it will test for the question text.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Start conversation')
    		->assertQuestion();
}

public function testBasicTest()
{
	$this->bot
    		->receives('Start conversation')
    		->assertQuestion('What do you want to talk about');
}
```

### assertTemplate
The `assertTemplate` method asserts that the chatbot does reply with a given template class. This is especially useful when testing Facebook reply templates.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('News feed')
    		->assertTemplate(GenericTemplate::class);
}
```

You can pass `true` as a second argument to test that the templates are exactly the same (strict mode).

```php
public function testStartJokeConversation()
{
	$jokeTypeQuestion = ButtonTemplate::create('Please select the kind of joke you wanna hear:')
				->addButtons([
					ElementButton::create('Chuck Norris')->type('payload')->payload('chucknorris'),
					ElementButton::create('Children')->type('payload')->payload('children'),
					ElementButton::create('Politics')->type('payload')->payload('politics'),
				]);
	
	$this->bot
		->receives('start joke conversation')
		->assertTemplate($jokeTypeQuestion, true);
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

<a id="testing-events"></a>
## Testing Events
BotMan supports testing for driver specific events with the `receivesEvent` method.

```php
public function testStartConversation()
{
	$this->bot
		->receivesEvent('event_name')
		->assertReplies('Received event');
}
```

You can pass a `$payload` array as an additional argument as well.

```php
public function testStartConversation()
{
	$this->bot
		->receivesEvent('event_name', ['key' => 'value'])
		->assertReplies('Received event);
}
```

<a id="advanced-testing"></a>
## Advanced Testing
### Set Driver
If you are building multi-driver chatbots with BotMan you may have restricted some commands to specific drivers only - for example using the `group` method.

```php
$botman->group(['driver' => FacebookDriver::class], function ($bot) {
    $bot->hears('Facebook only', function ($bot) {
        $bot->reply('You are on messenger');
    });
});
```

The BotMan testing environment uses a fake driver class to run the tests, but we can change the name of the driver with the `setDriver` method.
 
```php
public function testStartConversation()
{
	$this->bot
	   	->setDriver(FacebookDriver::class)
		->receives('Facebook only')
		->assertReply('You are on messenger');
}
```

### Set User Information
In a similar manner you can set user information with the `setUser` method.

```php
public function testStartConversation()
{
	$this->bot
		->setUser([
			'first_name' => 'John'
		])
		->receives('Hi')
		->assertReply('Hi, John!);
}
```

### Skip Assertion

In cases when your bot replies with several messages you can assert only a specific reply using the `skipReply` method to skip assertion. All tests below will pass.

```php
public function testStartConversation()
{
	$this->bot
		->receives('Tell me something')
		->assertReply('Hmm..')
		->assertReply('Ok')
		->assertQuestion('Here are some topics we could discuss');
}

public function testStartConversation()
{
	$this->bot
		->receives('Tell me something')
		->skipReply()
		->assertReply('Ok')
		->assertQuestion('Here are some topics we could discuss');
}

public function testStartConversation()
{
	$this->bot
		->receives('Tell me something')
		->skipReply(2)
		->assertQuestion('Here are some topics we could discuss');
}
```

### ReceiveRaw and AssertRaw

If none of the helper methods can handle your specific test case you can use the `receivesIncomingMessage` and `assertOutgoingMessage` methods.

```php
public function testStartConversation()
{
    $incoming = new IncomingMessage('Hi', '12345678', 'abcdefgh');
    $outgoing = new OutgoingMessage('Hello');
    
	$this->bot
		->receivesIncomingMessage($incoming)
		->assertOutgoingMessage($outgoing);
}
```

