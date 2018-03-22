---
layout: post
title:  "Visual Studio Project Templates for Ionic"
date:   2015-01-15 07:00:00 +0100
categories: software development
---
<img src="/assets/ionic/ionic-templates-for-visual-studio.png" style="width:100%">

**[UPDATE]**
**This extension was removed from Visual Studio Gallery in favor of [Ionic's official one](https://visualstudiogallery.msdn.microsoft.com/4e44ba8b-a4c8-4106-b70e-00d63241a54a).**

On my [previous post](({{ site.baseurl }}{% post_url 2015-01-08-ionic-creator %})) I wrote about how to start a project in Ionic Creator and export it to continue with development within Visual Studio.

Today, I would like to introduce the Ionic Project Template Visual Studio Extension that I created to simplify the process and be able to have a project already setup with the basic Ionic templates from Visual Studio <!--more--> (of course it is not a replacement for Ionic Creator, there you can drag and drop more components and set their style, but once you get familiar with Ionic, you would probably appreciate to be able to start the project quicker and apply the style yourself). You can find the extension in the [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/8fa5bff2-e023-4e13-8b36-0244e935fb7d).

Remember that **you need to have** the [Visual Studio Tools for Apache Cordova](http://www.microsoft.com/en-us/download/details.aspx?id=42675) installed first.

## How to Use the Extension
Once you have the Visual Studio Tools for Apache Cordova installed, and the Ionic Project Templates extension installed (remember to restart Visual Studio after installing the extension), just go to the File menu, then New, then New project.

 
The New Project dialog appears (as in the picture in the beginning of the post). Under Javascript you should now see the Ionic Project Templates installed. Just select the one you would like to start with and give it a name, a path where to save it, and optionally add it to source control as you would do with any other project.

 
## About the Templates
The templates will provide you the basic structure that you would get when exporting them from Ionic Creator. Well, maybe a little bit more. Some are tuned and with namings to help you understand what is what (like the tabs template). They also include the project structure (Ionic Creator only gives an index.html unless you export it using the Ionic CLI), with all their dependencies and the code adjusted to run with the Visual Studio Tools for Apache Cordova. Version 1.0 of the extension comes with Ionic's Beta 14.

 
There are 5 templates included in the VS Extension:

- Blank Ionic App
- A template with a built-in login page
- A template with a built-in side menu
- A template with a built-in signup page
- And a template with built-in tabs

## Collaborate
Do you want to improve the templates and keep the extension up to date? Feel free to [join the GitHub project](https://github.com/nereolopez/IonicProjectTemplates)! **[Update: was removed as said before to favor the official ones from the Ionic folks]**

 
In case you have comments or questions, feel free to leave a comment either here or in the Visual Studio Gallery [page](https://visualstudiogallery.msdn.microsoft.com/8fa5bff2-e023-4e13-8b36-0244e935fb7d). **[Update: was removed as said before to favor the official ones from the Ionic folks]**

 
Hope it helps!