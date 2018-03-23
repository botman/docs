# FAQs

- [What can I use BotMan for?](#what-can-i-use-botman-for)
- [Which Messenger drivers are supported?](#which-messenger-drivers-are-supported)
- [Can I create my own driver?](#can-i-create-my-own-driver)
- [Can I use NLP platforms like Wit.ai or Dialogflow?](#can-i-use-nlp-platforms-like-witai-or-dialogflow)
- [Does it work without Laravel?](#does-it-work-without-laravel)
- [Which PHP version do I need?](#which-php-version-do-i-need)
- [How does BotMan save the state of a conversation?](#how-does-botman-save-the-state-of-a-conversation)
- [Why when I change the webhook URL to something other than /botman does it stop working?](#change-web-hook-url)

<a id="what-can-i-use-botman-for"></a>
## What can I use BotMan for?

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, known as chatbots today. You can use it to build simple command-bots as well as advanced conversational interfaces. 

<a id="which-messenger-drivers-are-supported"></a>
## Which messenger drivers are supported?

Right now these messengers are included: Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger and WeChat.

<a id="can-i-create-my-own-driver"></a>
## Can I create my own driver?

Yes you can. Build your own driver to connect BotMan with other messengers or to use it as an API.

<a id="can-i-use-nlp-platforms-like-witai-or-dialogflow"></a>
## Can I use NLP platforms like Wit.ai or Dialogflow?

Both platforms are available through given middlewares. They help you to understand the user's message and provide information about their intent. You can of course create your own middleware.

<a id="does-it-work-without-laravel"></a>
## Does it work without Laravel?

As already mentioned BotMan is a framework agnostic PHP library. So yes, it works without Laravel too. If you do want use Laravel, checkout the [BotMan Studio](https://github.com/botman/studio) project.

<a id="which-php-version-do-i-need"></a>
## Which PHP version do I need?

BotMan requires PHP >=7.0.

<a id="how-does-botman-save-the-state-of-a-conversation"></a>
## How does BotMan save the state of a conversation?

BotMan provides a Conversation object that is used to string together several messages, including questions for the user, into a cohesive unit. In order to always know the current state of the conversation, the object gets saved through the cache.

If not specified otherwise, BotMan will use array cache which is non-persistent. When using the Laravel facade it will automatically use the Laravel Cache component. Symfony and Codeigniter caches are also supported.

<a id="change-web-hook-url"></a>
## Why when I change the webhook URL to something other than /botman does it stop working?

Laravel protects your application from cross-site request forgery (CSRF) attacks and as a result all requests are subject to a CSRF token verification. As the token is not passed along with the request from Facebook you need to exclude your webhook from CSRF checks. 

You can exclude the routes by adding their URIs to the $except property of the VerifyCsrfToken middleware.
