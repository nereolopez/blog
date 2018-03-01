---
layout: post
title:  "C# Classes"
date:   2018-02-20 07:00:00 +0100
categories: software development
---
# Class
A `class` allows us to create a our own custom Type by grouping together fields, methods and events. It is like creating a blueprint of an object where we can define its data and behavior.

```csharp
class User{
}
```

We've seen the code above already. Unless a class is declared as `static`, we need to create an instance of it and assign it to a variable to be able to use it. This will remain in memory until all references to it go out of scope.

## Constructor
A `constructor` is a block of code that is executed when instantiating a `class`. It can have one or several constructors. It's function is to allow us to set default values and limit instantiations.

If no `constructor` is provided, then a default one is created that creates a default instantiation of our `class` and sets the default values of its members according to their types.

```csharp
public User(){
    // this is the constructor of the Use class declared above
}
```
As you can see, to declare a `constructor` we just need to give it an Access Modifier, the same name of the `class` it will instantiate, and the parentheses `()` with the expected parameters if any. Note that it is like declaring a Method with the exception that it has no return type on its signature, and that its name will always be the same as the container `class`.

## Class Members
The members of a class tell us what is the data it can hold and what its behavior will be. We will see the following members of a class:
- Fields
- Constants
- Properties
- Methods
- Events

### Fields
Fields are nothing else than variables of a given type that are contained within a class. Generally we should use fields for variables that will be either `private` or `protected` (read more below on Access Modifiers). 

```csharp
private DateTime creationDate;
```

Fields can be marked as `readonly` on its declaration, which means that they can only be assigned a value during declaration or within a `constructor`.

*Note that fields are initialized immediately before the constructor.*

### Constants
Constants are values that are known at design time and will not change during execution. To create a `constant` we use the `const` keyword. There are **two limitations** that we have to keep in mind:
- We can only have constants with `fields` (never with `properties`, `methods` or `events`).
- The `const` keyword can only be used with C# built-in types (excluding `System.Object`), which means that our own types cannot be used as constants.

```csharp
class Calendar{
    const int months = 12; // constant
}
```

### Properties
Opposite to fields we will use Properties when willing to expose a member from our class and provide a flexible way to read, write or compute the value of a private `field`. As the provide access to data within the class they are also known as Accessors. 

This is how a `property looks like:

```csharp
public string Name {get; set;}
```

#### Auto-implemented Properties

It is like declaring a variable but by convention we will use Upper Camel Case and then it has this `get` and `set` keywords between curly brackets `{}`.

When using like that, the value that will be returned when accessing the `property` (hitting the `get`) will be its own value, and when assigning it a value (hitting the `set`) its own value will be overwritten. These type of Properties are known as **Auto-implemented Properties**.

#### Backing Fields Properties

A common pattern is to "hide" the value of `fields` (members that can be only accessed usually from within their containing `class`) inside of a `property`.

```csharp
private int seconds; // private field that will be hidden inside the Hours Property
public double Hours {
    get { return this.seconds / 3600 };
    set {
        this.seconds = value * 3600;
    }
}
```
You probably noticed the `value` keyword. Inside of it is where we can received the value that was assigned to the Hours `property`. Imagine we have an instance of the class (where the code above lives) in a variable called `timePeriod` and we want to use the `property` `Hours`:

```csharp
timePeriod.Hours; // will print the value of the seconds field multiplied by 3600;
timePeriod.Hours = 2; // will override the value of seconds with 2 multiplied by 3600
``` 

#### Expression Body Definitions
Remember that we saw that [when talking about Methods](https://nereolopezblog.wordpress.com/2018/02/08/c-methods/)? Well, we can do the same for both the `get` and `set` since C# 7.

The code above would look as follows:
```csharp
public double Hours{
    get => this.seconds / 3600;
    set => this.seconds = value * 3600;
}
```

### Methods
[We've already seen Methods](https://nereolopezblog.wordpress.com/2018/02/08/c-methods/), so we will only state here that we can create as many as needed within our classes. 

Maybe one word of advice would be that you should try to keep your methods short, doing only one thing (targeting only one goal) and giving them names as descriptive as possible.
 
### Events
We will not see them in detail now. We will just mention that Events are a way in how our class can notify other objects that something of interest has happen. The class or object raising the `event` is known as the `publisher` and the one that is listening to it to handle it is known as the `subscriber`.

You can read [more information on Events](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/events/index) in the official documentation from Microsoft if you feel like going for them now.

### Static Classes and Members
We've already seen throughout the code and the previous sessions the `static` keyword, but, what is it exactly? 

#### Static Classes

Let's focus when applying it to a class first.

When a class is marked as `static` it's only difference with a non-static class is that it cannot be instantiated. This means that we cannot use the `new` keyword to create a variable of our class type. This also means that, as there won't be any instance, we can use the members of the class by using its name instead.

If we take a look at the Math class that comes with the .Net Framework, we can see that there is no particular data that needs to be stored or retrieve for an instance of it, therefore, it is `static`. As said, to use any of its members we use the name of the class directly as follows:

```csharp
Math.Round(3.1415);
```

A Static Class has the following features:
- Contains only static members.
- Cannot be instantiated.
- It is sealed ([we will see later what this means]({{ site.baseurl }}{% post_url 2018-02-22-csharp-inheritance %}).
- Cannot contain instance constructors.

#### Static Member
Static members don't require to be within a static class. A static member on a class is callable even when no instance of the class has been created. This applies to all its types of members.

### this 
As a best practice, whenever you are refering to a member of your class, use the `this` keyword, which points to the object you are in.

```csharp
class User{
    private string name;

    public ShowName(){
        Console.WriteLine(this.name);
    }
}
```

## Using a Class
We've already seen it implicitly during the course, both how to use static and non-static classes. We just saw also how a static class works, so let's focus on non-static ones now that we now how to declare it together with its members. We will assume we are using the `User` class declared in the beginning of this article.

```csharp
// To instantiate it
User user = new User(); // we instantiate with the new keyword and calling its constructor
string username = user.Name; // we use the instance of the class to access its members
user.IsAdult(); // we use the instance of the class to call one of its non-static methods 
```

## Access Modifiers
We can define who is eligible to use our classes and their members by using on them Access Modifiers when declaring them. You'll recognize them now you see the list keywords below:
- **public**: the type or member can be access by anyone.
- **protected**: the type or member can be access by code in the same class or in a class derived from it.
- **private**: the type or member can only be accessed from the class itself.
- **internal**: the type or member can only be accessed by code in the same assembly.
- **protected internal**: the type or member can only be accessed by code in the same assembly, or from a derived class in another assembly.
- **private internal**: the type or member can only be accessed by code in the same assembly, by code in the same class or in a derived type.

*Note that to fully understand some of them you will need the information on the [session about inheritance]({{ site.baseurl }}{% post_url 2018-02-22-csharp-inheritance %}).*

## Classes Best Practices
When it comes to make code readable, you would probably want to order the code within your classes as follows:
- Members, Properties, Constructors, Methods
- Order each of the categories above by accessibility, from the more accessible (`public`) to the less one (`private`).
- Only one class (or object) per file.

# Exercises
1. Create a class representing the User profile from the [Variables and Types session exercises](https://nereolopezblog.wordpress.com/2018/01/30/c-variables-and-types/) and adapt the profile creation to use the class instead.  You must follow all the rules described in the exercise mentioned right now and also in the [Control Flow session exercises](https://nereolopezblog.wordpress.com/2018/01/31/c-control-flow/).