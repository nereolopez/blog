---
layout: post
title:  "C# Inheritance"
date:   2018-02-22 08:00:00 +0100
categories: software development
---
# Inheritance

Same way as you are smart, or good-looking, or good at sports, or good at music, have a nationality, a culture (and so on) in part because of you, and in part because you inherited those capabilities and attributes from your parents, the same can be applied in Object Oriented Programming. We saw one of the properties of OOP (encapsulation), and now we will learn about inheritance.

With inheritance we can create new classes that reuse, extend or even modify the behavior defined in another class.

## Derived Classes
Here comes the first difference with real life. You inherit talents and attributes from many people (parents, grand-parents...), but classes can only inherit from one class.  Here is how we specify that our class inherits from another one:

```csharp 
class DerivedClass : BaseClass{
}
```
*Note the `:` which specifies that the DerivedClass inherits from the BaseClass.*

The "parent" class is usually refert to as "base" class, and the one inheriting from it is known as "derived" class. When a class inherits from another, it means that it has by default the same attributes and capabilities as it can access everything in the base class. Well, **that's not true at all!**. Keep in mind that [Access Modifiers do exist as we've already seen in this session]() (not yet available). We will talk more on it soon.

Note the following two keywords:
- `sealed`
- `abstract`

These two keywords directly impact on class inheritance. 
>**A class marked as `sealed` is a class that cannot be used as  base class.** 
>**A class marked as `abstract` is a class that can only be used as a base class (which means it cannot be instantiated).**

```csharp
sealed class NotBaseClass { } // cannot be used as a base class
abstract class OnlyBaseClass { } // cannot be instantiated. Can only be used as base class
```

## Access Modifiers on Base Class
Keep in mind that the only Access Modifier that prevents a derived class from having access to something on the base class is the `private`.

```csharp
class BaseClass { 
    private string name;
}

class DerivedClass: BaseClass{ } // cannot access member "name" in the base class.
```
On the other hand, if you want that something can only be used either on the Base or Derived class, you have to mark it as `protected`.

## Overriding Members
As said before, by default we inherit everything as it is from the base class, but in the same way you inherit the Height attribute from your parents, but most likely not its value, or the same way you got that BBQ cooking skills, but you execute it different (you override their way of doing), you can override the Base class members. Important keywords when it comes to overriding:

- **virtual**: you mark base class' members as `virtual` to allow derived class to override them.
- **override**: you mark derived class' members with `override` to say that it will override the base class' member.
- **abstract**: when you mark a base class' member as `abstract` it **requires** that the derived class matching member overrides it.
- **new** Modifier: when marking a derived class' member as `new` it completely hides the member from the base class.

*See the [Code Sample]() (not yet available) for more details on this.*

## base Keyword
We've already seen `this` keyword when speaking about how to reference members of the class we are in. We could use it in Derived classes to refer to members of the Base class because Base class members become part of the Derived class too implicitly. But, if we use `base` instead is more meaningful where in the code what we are referring to is.

Usually, `virtual` methods provide a common ground to those all derived methods that will override it, meaning that in the beginning the derived methods might want to call it (though is not mandatory). 

```csharp
public override void MyMethod(){
    base.MyMethod(); // to make sure that common ground functionality is done on the base class
   
   // some other statements that are especific to the Derived class.
}
```

The same applies to the Constructor of a class, we might want to invoke the Base class' constructor first which initializes the object state and then initialize the things that are specific to the Derived class. We do it as follows:

```csharp
class Son: Mother {
    public Son : base() { }  // the :base() invokes the base class' constructor
}
```

# Exercises
You will have to decide how to apply all the concepts we discussed about inheritance, if you need abstract classes or members, what should be virtual, what should override, etc.

1. Create a Vehicle class with the following members:
    1. StartEngine()
    2. StopEngine()
    3. EngineStatus
    4. Speed
    5. NumberOfWheels
    6. Environment (road, sea, air).
    7. Accelerate(). Use overloads here
    8. Brake(). Use overloads here too.
    9. DisplayInfo()
2. Create the following classes with the logic mentioned above as might correspond:
    1. Car
    2. Bike
    3. Vessel
    4. Airplane
3. Add to the specific classes above additional methods that might be relevant