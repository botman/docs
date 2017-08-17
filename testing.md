# Testing

- [Introduction](#introduction)
- [Single Message Replies](#single-message-replies)
- [Type Indicators](#type-indicators)
- [Originating Messages](#originating-messages)
- [Send Low-Level requests](#sending-low-level-requests)

## Introduction

Like Laravel, BotMan is build with testing in mind. Every part of it is well tested and this is the only way to keep such a big project running properly.

But also your bots should be build with tests and this is why BotMan Studio will help you with that. Testing bots and conversation is very different from your other projects. This is why BotMan Studio provides lots of helper methods for you. This is the basic test example you will find in your `ExampleTest.php` file.

```php
public function testBasicTest()
    {
        $this->bot
            ->receives('Hi')
            ->assertReply('Hello!');
    }
```

## Testing Conversations

## Testing Questions

## Testing Facebook Templates
