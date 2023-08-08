# Natural Language Processing

- [Introduction](#introduction)
- [Dialogflow (previously known as API.ai)](#dialogflow)

<a id="introduction"></a>
## Introduction

Natural Language Processing - or short "NLP" - is the process of extracting structured data from a sentence or paragraph.
The information is usually separated into `intents` and `entities`.

For example, NLP tools take a sentence like this:

```
Hey Calendar, schedule dinner with Nicole at 8 pm tomorrow.
```

and returns structured data that BotMan can process:

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
Instead you can make use of BotMan's middleware support and retrieve information from third party services like wit.ai or Dialogflow.

<a id="dialogflow"></a>
## Dialogflow

BotMan has built-in support for the [Dialogflow](https://dialogflow.com/) NLP service.
Please refer to the [Dialogflow documentation](https://dialogflow.com/docs/getting-started/basics) on how to set up your account and get started with the service.

To listen in BotMan for certain Dialogflow actions, you can use the ApiAi middleware class.

```php
use BotMan\BotMan\Middleware\ApiAi;

$dialogflow = ApiAi::create('your-api-ai-token')->listenForAction();

$botman = resolve('botman');
// Apply global "received" middleware
$botman->middleware->received($dialogflow);

// Apply matching middleware per hears command
$botman->hears('my_api_action', function (BotMan $bot) {
    // The incoming message matched the "my_api_action" on Dialogflow
    // Retrieve Dialogflow information:
    $extras = $bot->getMessage()->getExtras();
    $apiReply = $extras['apiReply'];
    $apiAction = $extras['apiAction'];
    $apiIntent = $extras['apiIntent'];
    
    $bot->reply("this is my reply");
})->middleware($dialogflow);
```
