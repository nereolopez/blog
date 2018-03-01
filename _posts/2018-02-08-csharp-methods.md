---
layout: post
title:  "C# Methods"
date:   2018-02-08 08:00:00 +0100
categories: software development
---
A method is a code of block that containes one or more statement. Every executed instruction in C# is run within the context of a method. Methods can be declared in a `class` or a `struct`.

## Method Signature ##
To declarel a method we need the following elements in the following order:
- **Access Modifiers**: `public`, `private`... [We will discuss more on this later]({{ site.baseurl }}{% post_url 2018-02-20-csharp-classes %}).
- **Type of return**: if the method does not return any value then it will be `void`, otherwise the type of the value we want to return (`int`, `bool`...).
- **MethodName**: this is up to you. You can specify the name you like the most for the method. Make sure that it reflects what the method does.
- **(parameters)**: the method name is followed by parentheses `()` than can have parameters inside.To specify that whoever calls our method shall pass us some arguments, we define the parameters by specifying their type and giving them a name.

There are few more things we can add to the signature of a method, but [we will see them later]({{ site.baseurl }}{% post_url 2018-02-22-csharp-inheritance %}).

```csharp
public void Greet(string name){ // this line is the signature of our Greet method
    Console.WriteLine("Hello {0}", name);
}
```

## Invoking a Method
To invoke a method we use its name, the parentheses and pass in the expected arguments). That works if the method is in the same class we want to invoke it from. If it is in a different object, then we need to use the instance of that other object followed by a dot `.`. Let's see both situations with the Greet method we used above: 

```csharp
randomClassInstance.Greet("Bob"); // this is assuming the Greet method is in a different class.
Greet("Bob"); // this is in the case the Greet method is in the same class we are working.
```

## Returning Values
If we say our method will give back any value, we do it by using the `return` keyword. 

```csharp
public int Add(int a, int b){
    return a + b;
}
```

Note that the type of what we are returning must match the returning type specified in our method's signature. The sample above is very simple, but, if you have a method where the execution flow is not sequential (like if you have any `if` or `switch`), a good practice is to avoid returning values from within their blocks and only have one reteurn point in the method, which should be the very last thing we do inside of it.

## Optional Parameters
We can specify parameters that are not mandatory for the code that invokes our function. These are called Optional Parameters. They must follow the rules below:
- Must have a default value as part of its definition
- When no argument is passed for an optional parameter, then the default value is used.
- Optinal Parameters must be defined after all the mandatory ones.
- They are not exclusive of Methods. Can be used in other places like in Constructors.

To mark a parameter as optional we assign it a default value in the method declaration using the `=` operator. 
```csharp
public void PrintPatient(string name, bool isAlive = true){

}
```

## Named Parameters
When passing arguments we may want to name them. One obvious reason is for the sake of making the code more readable. Another reason is that, if we have several Optional Parameters and we don't want to provide all of them, by using Named Parameters we can target the ones we want to provide specifically.

Imagine we have a function declared like this

```csharp
public void RegisterUser(string name, string lastname = null, string country = null){ }
```

To use a Named Parameter we just put the name of the parameter followed by a colon `:`. In the sample above we will use Named Parameters to skip the first Optional Parameter specified in the sample above (`lastname`).

```csharp
RegisterUser("Mike", country: "Canada");
```

## Expression Body Definitions

Oftentimes we'll find methods that immediately return with the result of an expresion or that simply have just one statement. We can use the following syntax shortcut using the arrow `=>`. The Add method above is perfect to show this:

```csharp
public int Add(int a, int b) => a + b;
```

*Note that if the return type is `void` or is an async method then the body of the method must be a statement expression.*

## Overloads
You have probably noticed that when using some functions already, intellisense was offering you several choices allowing you to navigate up and down with your arrow keys to see the different overloads of a method. Did not notice? Then
1. Write "Console.WriteLine(" inside of a function and wait for the Intellisense to open.
2. See that the tooltip that appears starts with an arrow up,"1 of 19", an arrow down, and then the signature of the first overload of the `WriteLine()`method. 
3. Use the up and down keys to navigate through all the different overloads of the method, and notice how they change in `Type` and number of parameters.

That is exactly an overload on a method. The possibility to create it several times with the same name, but with different number and/or types of parameters. Going back to our `Console.WriteLine()` method, thanks to overloading we can invoke it passing a `string`, a `bool`, an `int` and many other options and it will work anyway!

## Local Functions
We will not discuss them here, but FYI from C# 7 there are Local Functions. They are nothing else than functions within another member (usually Functions, but could be inside of a Constructor, of a Property accessor,  Event accessor...).

Their signature is the same as a regular function but without the Access Modifier because they are always private. Because Local Functions live inside other members, they can only be called from within their container.

```csharp
public void Greet(string name){
    PrintOnScreen(name);

    void PrintOnScreen(string name){
        Console.WriteLine(name);
    }
}
```

## Async
We mention it here just to be aware that we can make Functions to run code asynchronously, but we will discuss it later in our [Async Programming session]() (not yet available).

## Exercises
1. Create a main function that:
    1. Fills up an array of 5 numbers getting the numbers from a function that generates them randomly between 0 and 100.
    2. Calls a function that given an array (no matter its size) displays all its elements. 
    3. Gets the average of the generated numbers from a function that given an array of numbers (no matter its length) returns the average of all o f its elements. Then the main function displays the average.
2. Optional and Named Parameters: Create a main function that:
    1. Asks the user if her profile will be public, her name and her last name.
    2. Asks the user if she wants her name to be displayed and if she wants her last name to be displayed.
    3. Then main function calls another one that receives these three parameters and that displays them based on if they were passed or not.
3.  Method Overloading. Create a main function to define a Car Search:
    1. It shall be possible to only specify the Brand
    2. It shall be possible to specify the Brand and Model
    3. It shall be possible to specify the Brand, Model and if it is Diesel.
    4. It shall be possible to specify the Brand and if it is Diesel.
    5. It shall be possible to specify the Brand, if it is Diesel and the Year.
    6. It shall be possible to specify the Brand and the maximum desired Kms.