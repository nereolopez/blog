---
layout: post
title:  "Ionic Creator and Visual Studio"
date:   2015-01-08 07:00:00 +0100
categories: software development
---
<img src="/assets/ionic/ionic-creator.png" style="width:100%">

This time I would like to share how to easily start from scratch an Hybrid App using Ionic Framework, AngularJS and Apache Cordova. We will have two main tools for this guide: <b><a href="https://creator.ionic.io/">Ionic Creator</a></b> and Visual Studio with Apache Cordova Tools installed.

<!--more-->

## Ionic Creator

The guys from Ionic Framework are doing a great job and developing a new framework and backend services with a very good result in a record time. They've been running for about a year and it is impressive what they've done.

Ionic Creator is not as Ms Blend for XAML apps, it is more like the Visual Editor you find in almost every IDE.Then, what is especial about it? In the Ionic guys words: "<b>Lets you rapidly mockup and prototype a real mobile app in minutes</b>". More than that, you can just export it as a project and start working on the code and do the needed bindings. And that's it.

## Start the project
Go to the <b><a href="https://creator.ionic.io/">Ionic Creator</a></b> site and log in (sign up if you do not have an account). Now, we will create a project. Just give it a name and select the template you want to use.

![ionic-creator-new-project.PNG]({{"/assets/ionic/ionic-creator-new-project.png"}})

Look at the picture below. This is how it should look like. On the Left side you have the Pages pane, where your Tabs hierarchy in this case was created. Below, you have the available components to drag to the content area (located in the center of the screen, you will recognize it as it looks like an iPhone screen). On the Right side you will find the Properties pane. It will display the properties of the selected element. Just above it, you will see a switch between Edit and Test. This will allow you to switch between edition mode (the one we are now) and the "preview" mode, to see how what we've done would look like.

![ionic-creator-web.PNG]({{"/assets/ionic/ionic-creator-web.png"}})

## Creating the UI 
On the Pages pane open the Tabs node, and for every Tab node remove everything but its Content node. We only need two tabs for the sample, so remove the third one. Now, we are going to change their title and icon. For doing this, click on one Tab node. Notice that the Properties pane has been updated and now you can enter a title and select an icon. I called them "Attendees" and "Register", and selected the "ion-ios7-contact-outline" and "ion-ios7-bookmarks" icons respectively.

If you toggle to Test, you should see something like this:

![ionic-creator-test-attendees.PNG]({{"/assets/ionic/ionic-creator-test-attendees-1.png"}})

### Adding Extra Pages
Go to the Pages pane and click on the plus to add a new one and select the Blank template. Change its Title to "Attendees" and its Routing URL to "/attendees". Note that now that the Attendees page is selected, if you enter the Test mode you will preview this page, but if you have the Tabs page selected, you will preview the Tabs page instead.

Do exactly the same steps now for creating the "Register" page, but instead of selecting the Blank template use the Sign Up one. You will see a form with Name, Username and Password fields and a button to Sign Up. Feel free to play around with the style of the form (for instance, change the Type attribute of the Username to Email and the Type attribute of the Password to Password), but don't forget to rename the Routing URL to "/register"!

### Adding Content to the Pages
As our Register page is already fine, let's add content to our Attendees one. We want to see a list with all the Attendees the Workshop will have. Select the Attendees page in the Pages pane and, from the Components list, drag and drop into the View a List.

Now, select the Item Thumbnail component and drop it in the List you just dropped in the View. Do it several times. Now we have a strange list with a mix of regular list items and thumbnails list items. In the view, select the regular list items and delete them using the "x" that appears on the top-right corner. Change the properties of the thumbnails to make them appear like they have data. It could end up looking something similar to this:

![ionic-creator-test-attendees.PNG]({{"/assets/ionic/ionic-creator-test-attendees-2.png"}})

## Starting to Code
### Exporting from Ionic Creator
Now, on the header bar of the Ionic Creator, click on the second button (circled arrow down) that is right beside you project's name. This is the Export button. It will allow you to get your project in three different ways: using the CLI, downloading a ZIP or just copying the code. Use the ZIP option so we can use it later.

### Getting Visual Studio Ready
If you didn't do it before, you will need to install the <b><a href="http://www.visualstudio.com/en-us/explore/dn841948#Fragment_Quickstart">Visual Studio tools for Apache Cordova</a></b>. It will install <b><a href="http://msdn.microsoft.com/en-us/library/dn757054.aspx#InstallTools">a lot of stuff</a></b> for you.

Once it is installed, you will have a new project template under the Javascript language. Open Visual Studio, and create a new project for Javascript -> Apache Cordova Apps. See the picture below:

![vs-taco.PNG]({{"/assets/ionic/vs-taco.png"}})

### Bringing the Ionic Project into Visual Studio 
Now, it is time to bring what we've done in Ionic Creator into Visual Studio. Open the index.html file that Visual Studio created for you and now let's merge it together with the one Ionic Creator generated.
From Ionic Creator's index.html file, copy the following things to the Visual Studio's index.html file:

- Within the **`<head>`**:
    - The meta viewport tag.
	- The style for google map container (only if you will need it. If so, I would suggest to move it to your css folder instead anyway)
	- Import the references to ionic.css and ionic.bundle.js (you don't have them yet and Visual Studio will warn you. We will fix it later)
	- Add a reference to app.js (which does not exist yet)
- From the **`<script>`** tag:
    - In Visual Studio, create an app.js file and copy there all the javascript content from `<script>` tag in the index.html that Ionic Creator generated.
- Within the **`<body>`**:
    - Just copy the `<body>` itself with all its content. **Be careful! You will still need the two references (cordova.js and platformOverrides.js) that Visual Studio created for you within the `<body>` tag. Make sure you keep them!**

Now it is time to download the missing things:
- <b><a href="http://ionicframework.com/">ionic.css</a></b> (click the download button)
- ionic.bundle.js (comes in the zip from the link above):
- cordova.js (there is nothing to do with this. It will be created by Visual Studio the first time you succesfully compile).

<b>Important!</b> If you take a look at the index.html file you will notice that the references are expecting the ionic.css and ionic.bundle.js in the "lib" folder, which does not exist. Create it and copy the files there, or organize them as you want (but update the reference path in the index.html!)

If you run the project using Ripple, you should see something like this:

![vs-taco-ripple.PNG]({{"/assets/ionic/vs-taco-ripple.png"}})

I agree with you, not very impressive so far. Let's try to get our two pages within the two tabs, to see the name of the application in the header and to give a little bit of color. But first, let's do it understanding what was autogenerated.

### What is the Generated Code
In the `<body>` tag, the first thing you will notice is the `<ion-nav-bar>` and the `<ion-nav-view>`. Remove both leaving the parent `<div>` empty, as we are going to move some content here.

You will also find 3 `<script>` tags with an `Id` and a `Typep` (templates in this case). The `Id` is the name of the page. Take the content of the first one (`<ion-tabs>`) and place it within the `<div>` we just emptied. The `<script>` that is now empty, can be completely removed.

The `<ion-tabs>` has 2 `<ion-tab>`. Each of them has an `<ion-content>` tag. Replace them with `<ion-nav-view name="">`. As name, for the first one put "attendees" and for the second one "register".

You have to also add the attribute `ui-sref` for every `ion-tab` like this:

```html
<ion-tab title=”Attendees” icon=”ion-ios7-contact-outline” ui-sref=”attendees”>

<ion-tab title=”Register” icon=”ion-ios7-bookmarks” ui-sref=”register”>
```

The `ui-sref` attribute points to a state configured in the AngularUI Router, we will see it in the app.js file.

For the other 2 `<script>` tags, rename their Id to "attendeespage.html" and "registerpage.html".

Go to the app.js file. It is creating and angular module called "app" with Ionic as a dependency. The next thing you will see is the ".config". Ionic uses AngularUI Router and, as we said, uses the concept of states. Defining states is the next thing you find. You probably already noticed that the tamplates URLs are not correct because we changed them in the index.html file. Let's remove the one that belongs to the tabs (as it is not a template anymore, but our main view container) and change the other two accordingly to "attendeespage.html" and "registerpage.html".

Let's do a short explanation here on states:
- they have a name (the first string after the opening parenthesis). Remember the state we are pointing to with the `ui-sref` attribute?
- they have a URL. It helps us to create the nested navigation if needed (and even indicate that parameters will be passed). In this case, we don't have that scenario, so we forget about it.
- the templateUrl is the Id given to the template we want to use to represent the state graphycally.

Yes, you got it right. we need to do something with the names of the auto-generated states. Call them "attendees" and "register". In this way, the <b>states</b> names will match the `ui-sref` attributes of the `ion-tabs`.

The URLs will remain as they are (as we gave them the name already in Ionic Creator).

The last change is the "templateUrl". The "attendees" state would completely look as follows:

```js
.state(‘attendees‘, {
    url: ‘/attendees‘,
    views: {
        attendees: {
            templateUrl: ‘attendeespage.html‘
        }
    }
})
```

If you remember, we said in one `ion-tab` that it would contain an `ion-nav-view` named "attendees", as the <b>view</b> we just defined for the "attendees" <b>state</b>. And we are saying that the <b>template 
</b> we will use for displaying that view is the content of the <b>script</b> with Id "attendeespage.html".

## Conclusion
This is the result we've got so far.

![vs-taco-ripple-attendees.PNG]({{"/assets/ionic/vs-taco-ripple-attendees.png"}})
![vs-taco-ripple-register.PNG]({{"/assets/ionic/vs-taco-ripple-register.png"}})

Now, just go ahead and play with the style, with angular, remove the fake data and do actual bindings, organize the code a little bit better than we've done, add some Apache Cordova plugins to access the hardware, connect to backend services like Firebase or Azure Mobile Services, and, of course, deploy it to the different app stores. If you do it, come back and let us know what you've created ;)

Hope it helps!

## Special Mention
As I used them for the sample app, I want to give special mention to my friend <a href="http://www.drewsimmie.com/"><b>Drew Simmie</b></a>, a great Personal Development Coach settled in Toronto. He is working close with the goverment to help young enterprenours to learn and act. You have to meet him. He will just touch you and inspire you.

And, of course, to my lovely wife, Denisse. She is just the best thing that could have happened in my life. I bet you will <a href="https://denissegomezdesign.wordpress.com/"><b>like her designs</b></a>!
