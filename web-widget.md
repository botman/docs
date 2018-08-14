# Web Widget

- [Introduction](#introduction)
- [Installation & Setup](#installation-setup)
- [Configuration & Customization](#configuration)
- [API](#api)

<a id="introduction"></a>
## Introduction

To make it as easy as possible for you, to add a BotMan powered chatbot into your website, BotMan ships with a custom chat widget that you can use out of the box. It uses Preact under the hood, which results in a minimal footprint of only 5kb (gzipped).

The web widget currently supports buttons, texts, images, videos, audio responses and the Facebook list- and button-template, so that you can just use the web widget, without modifying the code - even if it is specific to a messenger service.   

You can also see it running here in the documentation - most of the code samples work with the web driver, so feel free to just give it a try!

<a id="installation-setup"></a>
## Installation & Setup

The BotMan Web Widget is a frontend widget to use in combination with the [Web Driver](/__version__/driver-web). Make sure you install the web driver first, before you add the web widget to your website.

### Usage with BotMan Studio

If you use BotMan Studio - using the web widget could not be simpler. It already includes the web driver which comes with a view and route to use in combination with the web widget.
Simply add the Javascript snippet to your website:

```html
<script src='https://cdn.jsdelivr.net/npm/botman-web-widget@0/build/js/widget.js'></script>
```

That's it. You will now see a message icon in the bottom right corner. When you click it, it will open the chat widget.

> {callout-info} The script is now loading the chat iFrame from the `botman/chat` endpoint, which is automatically being added by the web-driver.

### Usage without BotMan Studio

If you are not using BotMan Studio and want to make use of the BotMan web widget, there are 2 steps involved.

#### Chat Frame 
When you open the web widget, it will load an iframe containing the chat area. In order to load this iframe, create a URL / route that is accessible from your application and contains the chat frame sourcecode:

```html
<!doctype html>
<html>
<head>
    <title>BotMan Widget</title>
    <meta charset="UTF-8">
    <link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/botman-web-widget@0/build/assets/css/chat.min.css">
</head>
<body>
<script id="botmanWidget" src='https://cdn.jsdelivr.net/npm/botman-web-widget@0/build/js/chat.js'></script>
</body>
</html>

```

#### Widget
Next you need to add the widget to your website and tell it to use the chat frame URL that you just created.
Simply add the Javascript to your website and configure it using a global config object. Place the URL that you created for the chat frame in the `frameEndpoint` configuration key.


```html
<script>
var botmanWidget = {
    frameEndpoint: '/iFrameUrl'    
};
</script>
<script src='https://cdn.jsdelivr.net/npm/botman-web-widget@0/build/js/widget.js'></script>
```

<a id="configuration"></a>
## Configuration & Customization

When adding the widget to your own website, you probably want to customize the look of the widget, the title that get's displayed in the widget bar, background images etc.
That's why the web widget comes with a detailed set of configuration options.

To apply these options to your widget, just declare an option called `botmanWidget` before loading the widget's Javascript.

The UI of the chat widget itself can simply be customized via CSS - take a look at the [default CSS](https://github.com/botman/web-widget/blob/master/src/assets/css/chat.css) to see the available selectors.

### Available settings

<table class="table" width="100%">
<thead>
    <tr>
        <th>Key</th>
        <th>Default</th>
        <th>Description</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>
            <pre>chatServer</pre>
        </td>
        <td>
            <pre>/botman</pre>
        </td>
        <td>
            The URL of the BotMan route / server to use.
        </td>
    </tr>
    <tr>
        <td>
            <pre>frameEndpoint</pre>
        </td>
        <td>
            <pre>/botman/chat</pre>
        </td>
        <td>
            The location of your chat frame URL / route.
        </td>
    </tr>
    <tr>
        <td>
            <pre>timeFormat</pre>
        </td>
        <td>
            <pre>HH:MM</pre>
        </td>
        <td>
            Time format to use
        </td>
    </tr>
    <tr>
        <td>
            <pre>dateTimeFormat</pre>
        </td>
        <td>
            <pre>m/d/yy HH:MM</pre>
        </td>
        <td>
            Date-Time format to use
        </td>
    </tr>
    <tr>
        <td>
            <pre>title</pre>
        </td>
        <td>
            <pre>BotMan Widget</pre>
        </td>
        <td>
            The title to use in the widget.
        </td>
    </tr>
    <tr>
        <td>
            <pre>introMessage</pre>
        </td>
        <td>
            <pre>null</pre>
        </td>
        <td>
            This is a welcome message that every new user sees when the widget is opened for the first time.
        </td>
    </tr>
    <tr>
        <td>
            <pre>placeholderText</pre>
        </td>
        <td>
            <pre>Send a message...</pre>
        </td>
        <td>
            Input placeholder text
        </td>
    </tr>
    <tr>
        <td>
            <pre>displayMessageTime</pre>
        </td>
        <td>
            <pre>true</pre>
        </td>
        <td>
            Determine if message times should be shown
        </td>
    </tr>
    <tr>
        <td>
            <pre>mainColor</pre>
        </td>
        <td>
            <pre>#408591</pre>
        </td>
        <td>
            The main color used in the widget header.
        </td>
    </tr>
    <tr>
        <td>
            <pre>bubbleBackground</pre>
        </td>
        <td>
            <pre>#408591</pre>
        </td>
        <td>
            The color to use for the bubble background.
        </td>
    </tr>
    <tr>
        <td>
            <pre>bubbleAvatarUrl</pre>
        </td>
        <td>
            <pre></pre>
        </td>
        <td>
            The image url to use in the chat bubble.
        </td>
    </tr>
    <tr>
        <td>
            <pre>desktopHeight</pre>
        </td>
        <td>
            <pre>450</pre>
        </td>
        <td>
            Height of the opened chat widget on desktops.
        </td>
    </tr>
    <tr>
        <td>
            <pre>desktopWidth</pre>
        </td>
        <td>
            <pre>370</pre>
        </td>
        <td>
            Width of the opened chat widget on desktops.
        </td>
    </tr>
    <tr>
        <td>
            <pre>mobileHeight</pre>
        </td>
        <td>
            <pre>100%</pre>
        </td>
        <td>
            Height of the opened chat widget on mobile.
        </td>
    </tr>
    <tr>
        <td>
            <pre>mobileWidth</pre>
        </td>
        <td>
            <pre>300</pre>
        </td>
        <td>
            Width of the opened chat widget on mobile.
        </td>
    </tr>
    <tr>
        <td>
            <pre>videoHeight</pre>
        </td>
        <td>
            <pre>160</pre>
        </td>
        <td>
            Height to use for embedded videos.
        </td>
    </tr>
    <tr>
        <td>
            <pre>aboutLink</pre>
        </td>
        <td>
            <pre>https://botman.io</pre>
        </td>
        <td>
            Link used for the "about" section in the widget footer.
        </td>
    </tr>
    <tr>
        <td>
            <pre>aboutText</pre>
        </td>
        <td>
            <pre>Powered by BotMan</pre>
        </td>
        <td>
            Text used for the "about" section in the widget footer.
        </td>
    </tr>
    <tr>
        <td>
            <pre>userId</pre>
        </td>
        <td>
            <pre></pre>
        </td>
        <td>
            Optional user-id that get's sent to BotMan. If no ID is given, a random id will be generated on each page-view.
        </td>
    </tr>
</tbody>
</table>

<a id="api"></a>
## API

The web widget also comes with an API to programatically send messages from the user or chatbot. You might use this feature when the user clicks on a button on your website to instantly trigger the chatbot, without having the user to type something.
Once the chat widget is initialized, you can access it's API on the `botmanChatWidget` object that get's exposed to the window containing the chat widget.

### Available Methods

<table class="table" width="100%">
<thead>
    <tr>
        <th>Method</th>
        <th>Description</th>
        <th></th>
    </tr>
</thead>
<tbody>
    <tr>
        <td>
            <pre>botmanChatWidget.open()</pre>
        </td>
        <td>
            Open the chat widget.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.open();return false;">Try out</a>
        </td>
    </tr>
    <tr>
        <td>
            <pre>botmanChatWidget.close()</pre>
        </td>
        <td>
            Close the chat widget.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.close();return false;">Try out</a>
        </td>
    </tr>
    <tr>
        <td>
            <pre>botmanChatWidget.toggle()</pre>
        </td>
        <td>
            Toggle the chat widget.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.toggle();return false;">Try out</a>
        </td>
    </tr>
    <tr>
        <td>
            <pre>botmanChatWidget.say(text)</pre>
        </td>
        <td>
            Say something on behalf of the user.<br>The given text is visible in the widget.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.say('Hi');return false;">Try out</a>
        </td>
    </tr>
    <tr>
        <td>
            <pre>botmanChatWidget.whisper(text)</pre>
        </td>
        <td>
            Similar to say, with the difference that the text is not visible.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.whisper('Hi');return false;">Try out</a>
        </td>
    </tr>
    <tr>
        <td>
            <pre>botmanChatWidget.sayAsBot(text)</pre>
        </td>
        <td>
            Say something on behalf of the chatbot.<br>The text is visible in the widget.
        </td>
        <td>
            <a href="#" class="btn" onclick="botmanChatWidget.sayAsBot('Hi, I am BotMan');return false;">Try out</a>
        </td>
    </tr>
</tbody>
</table>