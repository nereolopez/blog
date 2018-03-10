---
layout: post
title:  "C# Exception Handling"
date:   2018-02-08 07:00:00 +0100
categories: software development
---
So far we've only tested the "happy path". We always trusted that the user's input was going to be correct. We always trasted that casting would give no errors, but, that's not the way to go. 

An `Exception` is an error that happens at runtime and is propagated through the program. If nobody (no part of our code) handles that exception, then the execution of the program breaks. It is a class inside the `System` namespace. 

<!--more-->

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Exceptions.cs: where all the theory described here is shown.

## Exception Properties
- **InnerException**: gets the exception instance that caused the current exception.
- **Message**: gets a message that describes the current exception.
- **Source**: gets the object or application that caused the exception.
- **StackTrace**: gets the call stack.

# Handling Exceptions

## Try - Catch
A `try` block is used to wrap code that might be affected by an exception. Something like this:

```csharp
try {
    // some code that might break;
}
```

Well, to be honest, we don't do much there, Idealy, the `try` block will be followed one or several `catch` blocks.

```csharp
try{
    // code that might break;
}
catch (TypeOfExceptionToCatch ex){
    // now in 'ex' we captured the exception of the specified type and we can do something with it.
}
```

Each `catch` block consists of an `exception filter`. We need to declare the type of exception we want to intercept. The exception type should be derived from `Exception`, and, generally, we will not use `Exception` itself in a `catch`. The more specific the exception type we use the better.

## Finally
The `finally` block does not have to be present in all try-catch blocks. It is an opportunity to make some cleanup if needed. Imagine that you accessing a file and for some reason you do an action that throws an exception; then, in the finally, you will want to close the file.

```csharp
try{
    // code that might throw an exception
}
catch(IndexOutOfRangeException ex){
    // some exception handling code
}
finally {
    // code that has to be executed whether or not an exception ocurred
}
```

# Throwing Exceptions
As we can see in the samples provided in the code when doing something like the following

```csharp
var numbers = new int[5] { 1, 2, 3, 4, 5 };
Console.WriteLine(numbers[8]);
```

the framework itself throws an exception, letting us know that we are trying to access a position in the array that does not exist. That capability is very cool, and we can do it too.

To tell the application that there is an `exception` we want to raise we use the `throw` keyworkd. 

```csharp
if (name == null)
{
    throw new ArgumentException("Parameter cannot be null", "name");
}
```

## When to Throw an Exception
- The method cannot complete its define functionality (like when the data received by parameters is null).
- An innapropiate call to an object is made, based on the object state (like when trying to write in a file that was open in read only mode).
- When an argument to an object causes an exception (like in the example above with a non existing index).

## What Should Be Avoided
- Should not be used to change the flow of a program.
- Should not be returned as a return value or parameter.
- Do not throw `Exception`, `SystemException`, `NullReferenceException` or `IndexOutOfRangeException` from your source code.
- Do not create Exceptions that can be only thrown in debug mode but not in release mode.

## Creating Exception Classes
For now, let's say that it is possible for us to create our own Exception Classes. [Later on we will see classes]() (not available yet). Once we are familiar with classes we can start thinking about creating our own.