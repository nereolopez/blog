---
layout: post
title:  "C# Dependency Injection"
date:   2018-03-13 08:00:00 +0100
categories: software development
---
When we write our code, we usually find ourselves getting dependencies between objects. For example, if we have a class that handles some logic (let's refer to it as Business Logic class), but relays on another one who is responsible for getting data from the data source (let's call it Service class), then, we can say that the Business Logic class depends on the Service class. 

<!--more-->

Scenarios like the described above can end up in code similar to the below:

```csharp
public class BusinessLogic {
    private Service service;

    public BusinessLogic() {
        this.service = new Service();
    }
}
```

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- DependencyInjectionSamples.cs: where all the theory described here is shown.

## Tight-coupled Code
The code above is tight-coupled, the two objects have a very strong dependency between them. Imagine the Service needed some parameters to be initialized (even other constructed objects), then the Business Logic class would be responsible to know how to construct it, and not only that class, but anyone who would need the Service. Additionally, every time you make a change to it, we need to maintain all the dependent objects. 

You can easily spot such a thing by simply noting when you are using a `new` to create an instance of an object inside of another one.

## Loose-coupled Code
Instead of giving the responsibility to the dependent object to know exactly how the object it depends upon should be constructed, we can give it to it already set up, or, what is the same, passing the object it depends on already constructed, or, in other words, inject the dependency. 

There are several ways to inject a dependency, but the most common one is through the constructor. Let's rewrite the code above

```csharp
public class BusinessLogic {
    private Service service;

    public BusinessLogic(Service service) {
        this.service = service;
    }
}
```
Well, know we are injecting the dependency in the `constructor`. In this way, we don't put the responsibility to know how to create the Service in this case on the Business Logic class. If we get it already constructed, who's responsible for the dependency instantiation then?

## Dependency Injection Containers
There are several DI Containers out there in the wild that you can use (like [Unity](https://msdn.microsoft.com/en-us/library/dn223671(v=pandp.30).aspx), [Ninject](http://www.ninject.org/), [Autofac](https://autofac.org/)...). Nice, but, what are they for? Well, to put it simple, we will delegate to them by configuring the Container the instantiation of the dependencies. We will not go into details on this in this series.

Just the final words on DI is that, most likely, you will be (and you should) injecting dependencies in the form of an Interface (you'll understand one of the reasons on the session about [Unit Testing]() (not yet available) for instance). Using an Interface the code above would look as follows:

```csharp
public class BusinessLogic {
    private IService service;

    public BusinessLogic(IService service) {
        this.service = service;
    }
}
```