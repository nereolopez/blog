---
layout: post
title:  "Bot Framework Quick Start"
date:   2017-01-07 07:00:00 +0100
categories: software development
---
<img src="/assets/bot-framework/bot-framework-quick-start.png" style="width:100%">
Instead of having you reading all the official documentation, I hope this gives you a good starting point.

<!--more-->

## What is BotFramework?
It is a framework developed by Microsoft that allows devs to create bots that can interact with users in a natural way. It can be used in a website, app, Skype, Slack, Facebook Messenger, and many others.

## What can a bot do?
The idea behind is that the users of your application (whatever and wherever it is) will be able to get stuff done through a conversation with your bot. It is not only empowered to just give an answer, but to show rich information, propose actions, execute commands, etc. 

## How does it work?
Basically, with the open-source SDK we'll be using, we create a REST API that receives and sends back known models (schemas) that we will discuss later.

## What do I need to get started?
*Note that we are covering the .Net path, but it is also possible to do it with Node.js or via REST API.*

Only few things:
<img src="/assets/bot-framework/bot-framework-project-creation.png" style="width:100%">
- Visual Studio 2015 with all the extensions updated
- The BotFramework Template: [Download it](http://aka.ms/bf-bc-vstemplate) and put the zip in "%USERPROFILE%\Documents\Visual Studio 2015\Templates\ProjectTemplates\Visual C#\"
- Now in Visual Studio, when creating a new project you should see in C# templates the Bot Application template
- Download the [BotFramework Emulator](https://github.com/Microsoft/BotFramework-Emulator#download)

You could have more (app registration with Microsoft Bot Framework, Azure, etc), but to have your first hands-on, this is more than enough.

## Enough of talking, hands on!
Yes, sorry. Let's start. Follow along this steps and in 10 minutes you should have your first bot working.

### Create the Bot
Open Visual Studio and create a new project based on Bot Application template we just installed.

### The code you get
You get a fully functioning  Bot. The template as of the day of the writing of this article, in the controllers/messages controller, has a built-in Post method that receives an `Activity`. The only implemented path is the one where the `Activity` is of type `Message`. In this case, the code counts the number of characters of the `Message`'s Text and creates the `Activity` that will be sent back as an answer with the text telling the lenght of the message received.

In my case, I changed it a bit. As of today, it has a hardcoded (I know... that is a forbidden word!) greeting and a call to an external API to generate a random person identity, like this:

<img src="/assets/bot-framework/bot-framework-code.png" style="width:100%">

You can find the code on my [Github](https://github.com/nereolopez/fakeidentitybot) (but it might have evolved!)

### How to test it
Go ahead and run the application (in my case, Chrome is the default browser). You will see the following screen

<img src="/assets/bot-framework/bot-framework-simulator-1.png" style="width:100%">

Now, go ahead and open the BotFramework Emulator app. Click on the blue top bar and enter the URL where your application was launched (remember to include the port and to add "api/messages"), just like this:

<img src="/assets/bot-framework/bot-framework-simulator-2.png" style="width:100%">

As we did not register our app yet, leave the Microsoft App Id and Microsoft App Password fields empty, and click Connect:

<img src="/assets/bot-framework/bot-framework-simulator-3.png" style="width:100%">

Now you should be able to just enter a message in the text box that appears in the bottom and get the answer from your Bot with how many characters you have sent. You can normally debug your bot by setting breakpoints anywhere in your code, and the emulator gives you good hints on what errors might happen. A successful conversation should look like the picture below:

## Cool! Now, what?
That is a great question. Picture all the possibilities this brings you and your users. I can think of tons of useful use cases. Next step will be to dive a bit on what kind of features we can give to our bot (starting from the built-in we find in the SDK and extending it with Microsoft Cognitive Services), how to publish it and how to integrate it in different places.
 
As usual, any feedback is very welcomed. Hope you liked it!