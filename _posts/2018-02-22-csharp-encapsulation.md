---
layout: post
title:  "C# Encapsulation"
date:   2018-02-22 07:00:00 +0100
categories: software development
---
*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Encapsulation.cs: where all the theory described here is shown.

## Introduction
We already saw what a `Class` is, what its `Members` are and the Access Modifiers. Now let's combine the three of them to talk about Encapsulation.

We said before that objects have data and behavior. Now we will see how to procetc that data and control the access to certain parts of our objects by encapsulating them.

Imagine we have an object that represents our BankAccount, which has a decimal value for its Balance. If we do not protect the access to the Balance, anybody could change it in our BankAccount object, and for sure, that is not what we want.  To avoid unwanted modifications to the Balance, the first thing we want to do is to state that it can only be modified within our BankAccount object for instance, something like this:

```csharp
class BankAccount{
   private decimal balance;
}
```

In this way, if someone tries to access the balance would get an error as it is now hidden from the outside. But now that we guarantee that no one can change it from the outside, the implementation is not good enough, because, in fact, there are some external operations that can modify our balance, for example, when putting more money into our BankAccount. This means that we need a mechanism to modify the balance from outside, but not directly. In this case, we will use a method for that.

```csharp
class BankAccount{
    private decimal balance;

    public void PutMoney(decimal amount){
        this.balance += amount;
    }
}
```

Good. Now it is possible to update the balance in our BankAccount but not directly, we force and provide the right way to do it to make sure nobody changes the state of our object in an unexpected way. Now, what if it is needed to check the balance from the outside, like when going to the ATM? We need to provide some means for the ATM to access the balance of our account and show it to us. We will add a readonly property that exposes the internal balance:

```csharp
class BankAccount{
    private decimal balance;

    public decimal Balance => this.balance;

    public void PutMoney(decimal amount){
        this.balance += amount;
    }
}
```
