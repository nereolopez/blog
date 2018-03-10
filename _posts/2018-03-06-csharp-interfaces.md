---
layout: post
title:  "C# Interfaces"
date:   2018-03-06 08:00:00 +0100
categories: software development
---
An `interface` is like a `class` in the sense that allows us to define properties, methods and events, BUT (and is a big but), unlike a `class`, it does not contain any implementation. Then, who implements an `interface`? Classes implement Interfaces. We can think of an interface as a contract; when a class implements it, it has to contain every aspect of the interface.

Do you remember that multiple inheritance is not supported in C#? Well, luckily it is possible to implement more than one `interface` in a `class`, in that way, we can add to it functionality from different sources.

<!--more-->

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Interfaces.cs: where all the theory described here is shown.
- InterfaceExercises.cs: where all the exercises at the end of this post are solved.

## Creating an Interface

```csharp
interface IMyInterface {
    void MyMethod();
}
```
There are few things to mention here:
1. Notice the keyword `interface` to declare it.
2. I named it *IMyInterface* with capital *I* on purpose, it is not a typo. The reason is that, by convention, interfaces start with capital I so that it is easy to spot them.
3. There is no implementation of the method, we only declare it. Each declaration is a statement itself, therefore, ends with semicolon `;`.

## Implementing Interfaces
We said that interfaces have no implementation, but that they are implemented by classes. To say that a `class` implements an `interface`, we use the same syntax as when we want to inherit, with `:`.

```csharp
class MyClass : IMyInterface
{
    public void MyMethod() { } // method declared in the Interface!
}
```

Note that if we are inheriting from a Base Class and also implementing an interface, they should go both after the `:` separated by `,` and always writing the Base class first:

```csharp
class MyClass: MyBaseClass, IFirstInterface, ISecondInterface, IThirdInterface
{ }
```

Remember that we said that Interfaces and its members are public? This means that when implementing them they have to be public as shown above. Additionally, it has to be non-static and have the exact same signature as in the `interface`.

For properties, if the `interface` declares a property with only a `get` accessor, the `class` has to implement this property with the `get` accessor, but can add a `set` accessor too.

Keep in mind that Interfaces can implement other Interfaces. And we can use polymorphism with Interfaces. For example, if a Base Class implements an Interface, it can implement one of its members as `virtual`; in that way, the Derived Class can change the Interface behavior by overriding the `virtual` members of the Base Class.

## Interface vs Abstract classes
Interfaces are like Abstract classes in the sense that they contain declarations and cannot be instantiated. Additionally, `abstract` classes can provide partial implementation. Also keep in mind that C# does not allow multiple inheritance, but you can implement several Interfaces in one class.

## Exercises
1. Create a set of interfaces (having a base one) for Animals and the classification you consider the best, with the needed members (properties, methods...) for each of them.
2. Create one class per specific interface (not for the base interface) and implement the interface(s) it might need.
3. Create a small program that allows you to choose as a pet one of the animals you created a specific class for, to enter the details of your pet and interact with it (using its methods: things like feed, bath, go for a walk... of course, if it applies to the kind of pet you've chosen!).