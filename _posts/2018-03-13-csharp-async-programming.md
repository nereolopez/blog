---
layout: post
title:  "C# Async Programming"
date:   2018-03-13 07:00:00 +0100
categories: software development
---

Whenever you perform operations that might have a process that could make the user wait or leave the application blocked until the operation ends, such as network calls, database access and so on, you might want to use async programming.

<!--more-->

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- AsyncSamples.cs: where all the theory described here is shown.

## Overview

C# provides a language-level async programming model that allows you to handle async operations without all the inconvenience of callbacks.

The core of async programming are the `Task` and `Task<T>` objects which are supported by the `async` and `await` keywords.

The model is pretty simple:
- When it comes to I/O-bound code , you `await` an operation which returns a `Task` or `Task<T>` within an `async` method. See the sample below taken from the official documentation:

```csharp
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```

- When it comes to CPU-bound code, you await an operation which is started on a  background thread with the `Task.Run()` method. See the sample below taken from the official documentation:

```csharp
private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}


calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work.  The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

## Key Concepts
- Async code can be used for both I/O and CPU-bound code.
- `Task` and `Task<T>` are used in the background to model async work.
- `async` keyword turns a method into an async method which allows us to use the `await` keyword in its body.
- When the `await` keyword is hit, it suspends the calling method and yields control back to its caller until the async operation is completed.
- `await` can only be used within an `async` method.
- Add the **"Async"** suffix to the name of an `async` method by convention.
- Only use `async void` for Event Handlers as they don't have return type.

# Code Samples
Take a look at the code samples provided. We use an external library ([Json .Net from Newtonsoft](https://www.newtonsoft.com/json)). To get it into your project, right-click your project, select "Manage NuGet Packages...", make sure you are in the Browse tab of the view that is opened in Visual Studio, search for the Json package, select it and click the Install button.

![Newtonsoft Nuget.PNG]({{"/assets/newtonsoft-nuget.png"}})