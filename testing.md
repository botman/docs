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

Additionally you can you use the `assertReplies()` method.

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

BotMan can receive attachment objects from user and there are methods to test those cases. You can use `receivesLocation()`, `receivesImages`, `receivesVideos`, `receivesAudio` and `receivesFiles` methods to simulate that user has send valid attachments.

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
There are cases when we expect user to respond to question with interactive reply. To simulate that use `receivesInteractiveMessage` method.

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

`assertReply` asserts that bot reply with given message.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReply('Hello');
}
```

`assertReplies` asserts that bot reply with multiple messages.

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

`assertReplyIsNot` asserts that bot does not reply with given message.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReplyIsNot('Good bye');
}
```

`assertReplyIn` asserts that bot reply with message from given array. Helpful if your bot uses random answer generator.

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

`assertReplyNotIn` asserts that bot does not reply with any of given messages.

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

`assertReplyNothing` asserts that bot does not reply with any message. Useful when chained with other assertion methods.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('Hi')
    		->assertReply('Hello)
    		->assertReplyNothing();
}
```

`assertQuestion` asserts that bot reply with Question object.

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

`assertTemplate` asserts that bot does reply with given template class.

```php
public function testBasicTest()
{
	$this->bot
    		->receives('News feed')
    		->assertTemplate(GenericTemplate::class);
}
```

You can pass `true` to second argument to tests that templates are exactly the same.

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
BotMan support testing for events with `receivesEvent` method.

```php
public function testStartConversation()
{
	$this->bot
		->receivesEvent('event_name')
		->assertReplies('Received event');
}
```

You can pass `$payload` as additional argument as well.

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
If you are building multi-driver chatbot with BotMan you may have restricted some actions to specific drivers.

```php
$botman->group(['driver' => FacebookDriver::class], function ($bot) {
    $bot->hears('Facebook only', function ($bot) {
        $bot->reply('You are on messenger');
    });
});
```

BotMan testing environment uses FakeDriver class to run tests but we can change name of driver with `setDriver` method.
 
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
In similar manner you can set user information with `setUser` method.

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

In cases when your bot reply with several messages you can assert only specific reply using `reply` method to skip assertion. All tests below will pass.

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
		->reply()
		->assertReply('Ok')
		->assertQuestion('Here are some topics we could discuss');
}

public function testStartConversation()
{
	$this->bot
		->receives('Tell me something')
		->reply(2)
		->assertQuestion('Here are some topics we could discuss');
}
```

### ReceiveRaw and AssertRaw

If none of helper methods can handle your specific test case you can use `receiveRaw` and `assertRaw` methods.

```php
public function testStartConversation()
{
    $incoming = new IncomingMessage('Hi', '12345678', 'abcdefgh');
    $outgoing = new OutgoingMessage('Hello');
    
	$this->bot
		->receivesRaw($incoming)
		->assertRaw($outgoing);
}
```

