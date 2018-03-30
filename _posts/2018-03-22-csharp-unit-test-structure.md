---
layout: post
title:  "C# Unit Tests Structure"
date:   2018-03-22 07:00:00 +0100
categories: software development
---
We already saw an intro to Unit Testing. Now, we will focus on the structure a Unit Test itself will have and how to Setup a set of tests and Cleanup our Test Class. Theory will be short in order to focus on practicing.

<!--more-->
*Note that this post belongs to the [C# Unit Test Series]({{ site.baseurl }}{% post_url 2018-03-20-csharp-unit-test-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro). The relevant code for Unit Testing is in the folders that finish with the **.Tests** suffix. The classes they test are in their pair projects (for instance, the code that is tested in the *MyClasses.Tests* project is inside the *MyClasses* project).

## Unit Test Structure
You've probably noticed a pattern in the way we wrote the Unit Tests in [the Unit Test Introduction session]({{ site.baseurl }}{% post_url 2018-03-20-csharp-unit-test-intro %}).

Yes, we followed what is known as the **Triple A** which stands for **Arrange**, **Act** and **Assert**(others prefer to use Gherkins syntax instead with Give, When and Then).

All of our Unit Test need some sort or preparation (arrangement), then will act invoking the method where the current test scenario will be tested, and it will end with an assertion.

So, if we look at a known example, let's identify each block:
```csharp
public void CanWithdrawTest()
{
    // Arrange (or Given) 
    const decimal initialBalance = 1000;
    const decimal toWithdraw = 500;
    decimal expectedFinalBalance = initialBalance - toWithdraw;

    var account = new BankAccount();
    account.PutMoney(initialBalance);

    // Act (or When)
    account.Withdraw(toWithdraw);

    // Assert (or Then)
    Assert.AreEqual(account.Balance, expectedFinalBalance);
}
```

## Assertions
Notice that we could integrate third-party frameworks to our Unit Tests, but we will review the Assertions that come with Microsoft's Unit Testing Framework, in this case, inside the `Assert` class (you can check the full list of available assertion classes in the [Official Documentation](https://docs.microsoft.com/en-us/visualstudio/test/using-the-assert-classes)).
- **AreEqual(value1, value2)**: checks if the two provided values are equal.
- **AreNotEqual(value1, value2)**: the opposite the the method above.
- **AreNotSame(object1, object2)**: checks that two given object variables refer to different objects.
- **Equals(object1, object2)**: determines whether to objects are equal.
- **Fail()**: fails the assertion without checking anything.
- **Inconclusive()**: states that the assertion cannot be verified. 
- **IsFalse(value)**: verifies that the given condition is False.
- **IsInstanceOfType(object, type)**: checks that the given object is an instance of the given type.
- **IsNotInstanceOfType(object, type)**: checks that the given object is NOT an instance of the given type.
- **IsNotNull(object)**: checks that the given object is NOT null.
- **IsNull(object)**: checks that the given object is null.
- **IsTrue(value)**: verifies that the given condition is True.

## Setup and Cleanup
There might be cases where some variables or objects might be common to the Test Methods we have in our Test Class. In that case we can create a Method decorated with the `TestInitialize` attribute, which will force the execution of that method before the execution of the test method(s) to be run.

Opposite to it we have the `TestCleanup` attribute that will make a method decorated with it to be run after the execution of the test method(s). As you have guessed, we will use it to clean up our code when needed.

## Exercises

### Against Pure Functions
To practice simple unit testing, the best way is to do it against Methods that, given an input, return an output without the need of executing any dependency. Ideally, most of our code would look like that, but it is not the reality all the time. So, let's a class called "PureFunctions" inside of our  "MyClasses" Class Library project with the following methods inside that we will Unit Test in the corresponding file that we will create in our Unit Testing project (note that you will have to put into practice different kind of assertions):
1. **IsEven(int number)** which returns if the number passed as an argument is even or not.
2. **Divide(int a, int b)** returns the result of dividing a / b.
3. **IsInCollection(int[], int number)** returns if the given number is inside of the collection. Do not use the `Contains()` method, write the algorithm yourself instead.
4. **TransformCollectionNumbers(int[])** which will return the collection after transforming each of its elements. Even numbers will be devided by 2 and odd numbers will be multiplied by 2.

**Bonus:** If you feel like accepting the challenge, refactor the Exercise 1 of the [Collections session]({{ site.baseurl }}{% post_url 2018-02-13-csharp-collections %}) so that you can check it with Unit Testing.

### BankAccount
Let's add a new method on the BankAccount class we already have from the [previous session]({{ site.baseurl }}{% post_url 2018-03-20-csharp-unit-test-intro %}). The method will be `IsEligibleForCredit()` that will check based on a minimum amount in the Account's balance (30000). Create the Unit Test for that.

### Car Rental Agency
Yes, we have the Car Rental Agency application [we started here]({{ site.baseurl }}{% post_url 2018-02-23-csharp-first-app-part-I %}). Keep in mind that we iterated a bit over it on the [File System session]({{ site.baseurl }}{% post_url 2018-03-07-csharp-file-system %}) and [here too]({{ site.baseurl }}{% post_url 2018-03-15-csharp-first-app-part-iii %}) by adding File Storage, Interfaces and Injecting Dependencies (without using a Container).

As you know, so far the application writes some Fake Data to some text files for Cars and Customers. Note that we did not talk yet about [how to deal with dependencies in Unit Tests]()(not yet available), therefore, what we will do now will work, but is not the right approach to do it when it comes to Unit Testing, actually, it will resemble more to Integration Testing. Nevertheless, it will serve as an intro for that next session.

1. Create a Unit Test project for the Car Rental Agency and add the dependency to the CarRentalAgency project.
2. At `Car` level:
    1. Unit Test the the default properties initialization
    2. Unit Test the `Block()` method.
3. At `CarsManager` level:
    1. Check the `Cars` property
    2. Check the `AvailableCars` property

Note that some functionality is not written in a way (at this point) we could test (like `RentalsManager.AddRental()`. Maybe we'll come over it later. Also remember that we will discuss and do in the right way what we've just done in these two exercises.