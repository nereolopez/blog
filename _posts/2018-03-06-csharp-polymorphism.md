---
layout: post
title:  "C# Polymorphism"
date:   2018-03-06 07:00:00 +0100
categories: software development
---
*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Polymorphism.cs: where all the theory described here is shown.

> Polymorphism means that you can have multiple classes that can be used interchangeably, even though each class implements the same properties or methods in different ways.

That is the definition we find in the Microsoft official documentation of C#. But, how does it translates to or looks like in code? One key point it is mentioning is that multiple classes can implement the same properties or methods in different ways.

Polymorphism is the third characteristic we discuss about Object Oriented Programming (we've seen [Encapsulation]() not yet available) and [Inheritance]() (not yet available) already). To demonstrate this third characteristic, we will use some things we already know: inheritance, `virtual` methods and overriding methods.

```csharp
class Polymorphism
{
    public void PolymorphismSample()
    {
        // We will use the classes below in this file to demonstrate Polymorphism
        var lineTypes = new List<MyLine>();
        lineTypes.Add(new DottedLine()); // Note that we add DottedLine to a List<MyLine>
        lineTypes.Add(new DashedLine()); // which shows that classes can be used interchangeably

        foreach (var line in lineTypes)
        {
            line.Draw();
        }
    }
}

class MyLine
{
    public virtual void Draw()
   {
        Console.WriteLine("There is no generic line defined");
    }
}

class DottedLine: MyLine
{
    public override void Draw()
    {
        Console.WriteLine("......................................");
    }
}

class DashedLine: MyLine
{
    public override void Draw()
    {
        Console.WriteLine("--------------------------------------");
    }
}
```