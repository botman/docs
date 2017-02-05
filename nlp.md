# Natural Language Processing

- [Introduction](#introduction)
- [API.ai](#api-ai)

<a id="introduction"></a>
## Introduction

Natural Language Processing - or short "NLP" - is the process of extracting structured data from a sentence or paragraph.
The information is usually separated into `intents` and `entities`.

For example, NLP tools take a sentence like this:

```
Hey Calendar, schedule dinner with Nicole at 8 pm tomorrow.
```

and returns structured data like that BotMan can process:

```javascript
{
  "intent": "create_meeting",
  "entities": {
    "name" : "Dinner with Nicole",
    "invitees" : ["Nicole"],
    "time": "2017-02-05 20:00:00"
  }
}
```

Now this makes a perfect usage scenario for BotMan, as you are no longer tied to static text inputs that need to be exact matches.
Instead you can make use of BotMan's middleware support and retrieve information from third party services like wit.ai or api.ai.

<a id="api-ai"></a>
## API.ai

BotMan has built-in support for the [api.ai](http://api.ai) NLP service.
Please refer to the [API.ai documentation](https://docs.api.ai/page/guidelines) on how to set up your account and get started with the service.

To listen in BotMan for certain API.ai actions, you can use the ApiAi middleware class.

```php
use Mpociot\BotMan\Middleware\ApiAi;

$botman->hears('my_api_action', function($bot) {
	// The incoming message matched the "my_api_action" on API.ai
	// Retrieve API.ai information:
	$extras = $bot->getMessage()->getExtras();
	$apiReply = $extras['apiReply'];
	$apiAction = $extras['apiAction'];
	$apiIntent = $extras['apiIntent'];

	$bot->reply("this is my reply");
})->middleware(ApiAi::create('your-api-ai-token')->listenForAction());
```