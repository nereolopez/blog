---
layout: post
title:  C# Unit Testing With Dependencies
date:   2018-03-22 08:00:00 +0100
categories: software development
---
Oftentimes we will find ourselves in a situation where something we want to Unit Test has a dependency on another object. In the [previos session's exercises]({{ site.baseurl }}{% post_url 2018-03-22-csharp-unit-test-structure %}) we instantiated real dependency objects and passed them in, but that's not the idea of Unit Testing. How should we do it then?

We will use **Test Doubles**.  These are objects that pretend to be a real object for testing purposes. There are several types, here we will discuss **Stubs** and **Mocks**.

<!--more-->

*Note that this post belongs to the [C# Unit Test Series]({{ site.baseurl }}{% post_url 2018-03-20-csharp-unit-test-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro). The relevant code for Unit Testing is in the folders that finish with the **.Tests** suffix. The classes they test are in their pair projects (for instance, the code that is tested in the *MyClasses.Tests* project is inside the *MyClasses* project).

There is a lot of confusion and sometimes people refers to them as if they were the same, but let's put this simple statement:

*Mocks insist upon behavior while Stubs upon state.*

Among the reasons on why to use Test Doubles you can find really isolating the code under test, to keep consistent results (in our tests for the previous session for instance that were using real objects and not controlled File Storage, we were assuming that the Data Source was in a specific state, which might not be true and cause our tests most likely to fail).

Therefore, whenever an object we need to test has a dependency, we will use a Test Double for its dependency and inject it. The type of Test Double we select will depend on what we test. One of the reasons why it is better to do Dependency Injection using Interfaces is for Stubbing or Mocking.

## Design for Dependency Injection
Let me put here what the [Official Documentation](https://docs.microsoft.com/en-us/visualstudio/test/using-stubs-to-isolate-parts-of-your-application-from-each-other-for-unit-testing) states about that:

> [...] your application has to be designed so that the different components are not dependent on each other, but only dependent on interface definitions. Instead of being coupled at compile time, components are connected at run time. This pattern helps to make software that is robust and easy to update, because changes tend not to propagate across component boundaries. 

## Stubs
There are frameworks that provide us with the ability to create Stubs, but let's see the following actual code and then let's create our own Stub.

Imagine we have an `ICounter` interface
```csharp
public interface ICounter
{
    int Count { get; }
    void IncrementCount();
}
```

which is implemented by the `Counter` class:
```csharp
public class Counter : ICounter
{
    private int counter = 0;
   public int Count => this.counter;

    public void IncrementCount()
    {
        this.counter++;
    }
}
```

Then, we have the `OrderManager` that receives an `ICounter`. The `OrderManager` class is responsible for taking orders. Each time an order is taken, it is processed and then counter is incremented:
```csharp
public class OrderManager
{
    ICounter counter;

    public OrderManager(ICounter counter)
    {
        this.counter = counter;
    }

    public int PlaceOrderAndGetOrderNumber()
    {
        // Imagine some actions to process the order

        this.counter.IncrementCount();
        return this.counter.Count;
    }
}
```

Ok. Now that we have the situation, if we want to isolate the testing of the OrderManager we need a Test Double for the dependency (`ICounter` in this case). Let's say we have a test method to test that the order is correctly processed (whatever it means as that code is not done above, just a line commenting it there is something that should go on in that method), but also want to make sure that the method returns as an order number. That order number comes from the dependency, so we will need to setup the Stub and inject it. This would be our own Stub:
```csharp
class CounterStub : ICounter
    {
        private int count = 1234;
        public int Count => this.count;

        public void IncrementCount() { }
    }
```
Notice that we only care with Stubs about state, therefore, we leave the `IncrementCount()` without implementation. 

And this is how we would use it in our test:
```csharp
[TestMethod]
public void GetOrderNumberTest()
{
    // Arrante
    var stub = new CounterStub();
    int expected = stub.Count;
    var orderManager = new OrderManager(stub);

    // Act:  
    int actual = orderManager.PlaceOrderAndGetOrderNumber();

    // Assert:  
    Assert.AreEqual(actual, expected);
}
```

## Mocks
Even though Mocks put the stress upon behavior, we can setup everything on the pretended object. We will use **Moq** library (there are more Mocking libraries available out there).

We've already seen how to add NuGet packages to our project. In this case, we want to add the Moq library to our "MyClasses.Tests" Unit Test project, so right-click it -> Manage NuGet packages -> Browse -> Search for Moq -> Select it and install it.

![add-moq-nuget-package.png]({{"/assets/vs/add-moq-nuget-package.png"}})

For our example, let's create an `IFinancialService` interface in our *MyClasses* Class Library Project as follows:
```csharp
public interface IFinancialService
{
    string GetFinancialScore(decimal balance);
}
```

Now let's create the class that implements it:
```csharp
public class FinancialService : IFinancialService
{
    public string GetFinancialScore(decimal balance)
    {
        string score = string.Empty;
        switch (balance)
        {
            case decimal n when (n <= 1000):
                score = FinancialStatus.Bad.ToString();
                break;
            case decimal n when (n > 1000 && n <= 10000):
                score = FinancialStatus.Average.ToString();
                break;
            case decimal n when (n > 10000 && n <= 50000):
                score = FinancialStatus.Good.ToString();
                break;
            case decimal n when (n > 50000):
                score = FinancialStatus.Excellent.ToString();
                break;
        }
        return score;
    }
}
```

Note that it is using an enum called FinancialStatus (you will find it in the complete code [in the repo]() (not yet available)).

Our `BankAccount` class will now have a dependency on the `FinancialService` class. Therefore, we need to modify it. For that, we will create two constructors (one without dependency injection not to break our old tests - you would never do this in a real project - and another one for our new code that we will develop with a design oriented to Dependency Injection). below are only the lines that will be added to the `BankAccount` class, which are the field containing the dependency, the two constructors, and a new method that we will have to test:

````csharp
private IFinancialService financialService;

// Constructor added to avoid breaking the tests previous to the ones where we manage dependencies 
public BankAccount()
{
    this.financialService = new FinancialService();
}

public BankAccount(IFinancialService financialService)
{
    this.financialService = financialService;
}

public string GetFinancialScore()
{
    return this.financialService.GetFinancialScore(this.balance);
}
````

Ok. Now we have a new test in our Bank Account to be tested: the `GetFinancialScore()` method. Note that now the `BankAccount` class has a dependency on `IFinancialService` which is invoked in the method we want to test. Therefore, we will create a `Mock<IFinancialService>` in our test and would look as follows:
```csharp
[TestMethod]
public void GetFinancialScoreTests()
{
    // Arrange
    decimal balance = 1000m;
    expected string = "Bad";

    var mockFinancialService = new Mock<IFinancialService>();
    mockFinancialService.Setup(x => x.GetFinancialScore(balance)).Returns(expected);

    var account = new BankAccount(mockFinancialService.Object);
    account.PutMoney(balance);

    // Act
    string actual = account.GetFinancialScore();

    // Assert
    Assert.AreEqual(actual, expected);
}
```
As you can see, we first create a Mock instance strongly typed to the dependency we need to inject, then we set up what will be used in the code under test, and then we inject it in the *testee* that expects it as a dependency.

# Exercises
From the previous session we have two Unit Tests for our Car Rental Agency app that test the `CarsManager` class. Let's create two additional ones that will test exactly the same, but, this time, using Mocks instead of instantiated objects in our tests.