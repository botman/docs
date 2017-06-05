# FAQs

- [What can I use BotMan for?](#what-can-i-use-botman-for)
- [Which Messenger drivers are supported?](#which-messenger-drivers-are-supported)
- [Can I create my own driver?](#can-i-create-my-own-driver)
- [Can I use NLP platforms like Wit.ai or Api.ai?](#can-i-use-nlp-platforms-like-witai-or-apiai)
- [Does it work without Laravel?](#does-it-work-without-laravel)
- [Which PHP version do I need?](#which-php-version-do-i-need)
- [Can I add a Facebook persistent menu?](#can-i-add-a-facebook-persistent-menu)
- [How does BotMan saves the state of a conversation?](#how-does-botman-saves-the-state-of-a-conversation)

<a id="what-can-i-use-botman-for"></a>
## What can I use BotMan for?

BotMan is a framework agnostic PHP library that is designed to simplify the task of developing innovative bots for multiple messaging platforms, known as chatbots today. You can use it to build simple command-bots as well as advanced conversational interfaces. 

<a id="which-messenger-drivers-are-supported"></a>
## Which messenger drivers are supported?

Right now these messengers are included: Slack, Telegram, Microsoft Bot Framework, Nexmo, HipChat, Facebook Messenger and WeChat.

<a id="can-i-create-my-own-driver"></a>
## Can I create my own driver?

Yes you can. Build your own driver to connect BotMan with other messengers or to use it as an API.

<a id="can-i-use-nlp-platforms-like-witai-or-apiai"></a>
## Can I use NLP platforms like Wit.ai or Api.ai?

Both platforms are available through given middlewares. They help you to understand the user's message and provide information about their intent. You can of course create your own middleware.

<a id="does-it-work-without-laravel"></a>
## Does it work without Laravel?

As already mentioned BotMan is a framework agnostic PHP library. So yes, it works without Laravel too. If you do want use Laravel, checkout the [BotMan Laravel Starter](https://github.com/mpociot/botman-laravel-starter) project.

<a id="which-php-version-do-i-need"></a>
## Which PHP version do I need?

BotMan requires PHP >=5.6.

<a id="can-i-add-a-facebook-persistent-menu"></a>
## Can I add a Facebook persistent menu?

Yes you can, but not with BotMan ;-) Adding a persistent Facebook menu is just [one HTTP request](https://developers.facebook.com/docs/messenger-platform/messenger-profile/persistent-menu) to the Facebook API. No need to implement it here.

<a id="how-does-botman-saves-the-state-of-a-conversation"></a>
## How does BotMan saves the state of a conversation?

BotMan provides a Conversation object that is used to string together several messages, including questions for the user, into a cohesive unit. In order to always know the current state of the conversation, the object gets saved through the cache.

If not specified otherwise, BotMan will use array cache which is non-persistent. When using the Laravel facade it will automatically use the Laravel Cache component. Symfony and Codeigniter caches are also supported.
