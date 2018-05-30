---
layout: post
title:  "Nereo Material"
date:   2018-05-30 07:00:00 +0100
categories: software development
tags: [Angular, Angular Material]
---
I just released this modest library with the intent to provide components that are considered in Material Design guidelines, but not implemented (at least, as of today) in Angular Material. 

<!--more-->
## The Library
It is an Angular project with Angular Material, which takes advantages of the [support for Libraries](https://github.com/angular/angular-cli/wiki/stories-create-library) in the latest release of Angular CLI.

The Library itself is what contains the different modules and components. You can include it in your projects as an `npm` dependency just by running the following command:

`npm install nereo-material`

You can also check the code in its [repo in Github](https://github.com/nereolopez/nereo-material). Additionally, I created a [sample project](https://github.com/nereolopez/nereo-material-demo) that demonstrates how to use the components in your application, and you can check the [live demo site](https://nereo-material-demo.firebaseapp.com/home) I prepared so that you get a glance of how it would look.

Of course, any contribution and suggestion is more than welcome!

## First Component
First Component included in this package is the Contextual Toolbar. According to Material Design, whenever you have any selection on your screen, the toolbar should adapt to the context and offer you the possible actions for the selection you have. To make it easy to understand, here is a picture:

![Contextual App bar](https://material.io/design/assets/1sej9Nb4bY284xcA-oF5k_mgyTMxWN_2q/topappbars-contextual.png)
*Image from [Material Design Documentation](https://material.io/design/components/app-bars-top.html#usage)*

Long story short, this component is a toolbar that, based on your bindings, will display the number of selected items and the actions that you need to. Each time an action is tapped / clicked, you will receive an event so that you can handle the action in your component.

You can see the [demo of the Contextual Toolbar](https://nereo-material-demo.firebaseapp.com/contextual-toolbar) and [read its documentation](https://www.npmjs.com/package/nereo-material#contextual-toolbar) for more info.

## Background
My team uses Angular and Angular Material for the frontend of our projects, and, oftentimes, when discussing some requirements, I miss some of the functionalities we are used to have in Android and that are not present in Angular Material at this stage. We are also considering exporting our Angular Components as Custom Elements now it is a thing in Angular, so that we can re-use them in other projects / technologies (even for the other teams that are using React instead).

As a Project Manager, I am left out when it comes to have fun with these things in the office, so, I decided to do it at home :) I started simple, with a small component that we needed, and used this side project to play with the support for libraries in the Angular CLI, and will use it also to be able to export them as Custom Elements (so I might add this export option to this project too).

I hope you find it useful!