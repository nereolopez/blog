---
layout: post
title:  C# Unit Testing Intro
date:   2018-03-20 08:00:00 +0100
categories: software development
---
There are multiple quality gates that we can add to our software. Having developers and QAs (Quality Assurance engineers) in one team often leads to think that quality is the responsibility of the latest group, which is not true.

Developers can add quality gates to the software too, for instance, through Unit Testing. It consists on writing code that will test the code the developer is working on. Imagine the as a developer you are continuing with the BankAccount class we used as an example when speaking about [Encapsulation]({{ site.baseurl }}{% post_url 2018-02-22-csharp-encapsulation %}). Now you are interested in writing the Withdraw method, which will receive the quantity the user wants to get from the account.

<!--more-->
```csharp
public void Withdraw(decimal amount){
    this.balance -= amount;
}
```

**Yes, you probably noticed an issue in the implementation above!** Our fellow QAs will probably have written a Test Case for this new feature that checks that it is not possible to withdraw more money than the amount available in the account. Of course they will manually test it, but we as developers can add a Unit Test that checks our function behaves as expected in such scenario.

Our unit test for this concrete scenario would take the following steps:
1. Instantiate the Bank account class to be able to test it
2. Set it a balance
3. Call the `Withdraw()` method passing an amount higher than the available balance.
4. Make the Unit Test fail if the balance was modified in such conditions instead of giving us an exception.

Of course, such a test against the code above will not pass, as our implementation is not checking if the amount to withdraw is higher than the actual balance.

Remember that Unit Tests can be run automatically in every build. This is great, because we can cover most of the Test Cases using Unit Tests and know, after every code change if something is broken.

## Unit Test Project
Visual Studio has Unit Test Project Templates that you can use. As you are guessing, Unit Tests are written in separate projects.

Right-click on your solution and go ahead to add a new project (or go ti **File** -> **New** -> **Project**). On the dialog that pops up, select **Test** on the left panel and select **Unit Test Project** (which will use one of the Microsoft Unit Test frameworks).

![unit-test-project-creation.png]({{"/assets/vs/unit-test-project-creation.png"}})

In this project you will have one file per Test Class, and you will have one Test Class per Class in your real code you want to test.

## Test Class and Test Method
Test Classes and Test Methods are normal classes and methods decorated with a `[TestClass]` attribute and a `[TestMethod]` attribute respectively.

As said, you will have one Test Class per class you want to target, but you might have more than Test Method per method you want to test. For instance, the `Withdraw()` method we wrote before will have at least two test Methods covering it: 
- One testing the happy path (when the user enters an amount that is available in the account) 
- and another one for the not allowed path (asking for more money than the user has).

## Test Explorer
Visual Studio has a panel were our Unit Tests are shown. After each build it checks for new Unit Tests (or removed ones) and updates the Test Explorer accordingly. The Test Explorer allows us to filter the view and to run all, a set, or just one of the available Unit Tests. 

Once the selected Unit Tests are Run, it shows their status, if Passed, Failed or if they were Skipped.

For those of you who are using Visual Studio Enterprise you have an awesome feature which is **Live Unit Testing**. After every build you do, it does not only update the Test Explorer, but it also runs all the unit tests in the background when enabled, which gives you Live feedback on the quality of your code!

## Code Coverage Analyzer
Visual Studio also provides a Code Coverage tool which shows us all our actual project with its classes and methods and the percentage of coverage upon them with our Unit Tests. When this is activated, we can even see in the code the paths inside of it that are covered by any unit tests and those which are not.

![code-coverage-pane.png]({{"/assets/vs/code-coverage-pane.png"}})
*Image from [Microsoft's Official Documentation](https://docs.microsoft.com/en-us/visualstudio/test/unit-test-basics#qa)*

## Our First Tests
Let's implement our first tests following the BankAccount example above. For that, we will right-click the Solution (or File -> New -> New Project) to add a new project, and add a **Class Library project** called "MyClasses" (you can remove the default Class1 that is created within the new project).

We have not seen Class Library projects yet in all the sessions so far. A Class Library project does not have any UI, is just code that we will reuse in other Libraries or Applications. As it has no UI, the only way to check our code works is through executing it from another application, or, what is better, through Unit Tests :)

1. Create a BankAccount class according to what we've seen above:
    1. With a `Balance` property backed by a `balance` field.
    2. With a `PutMoney()` method as seen in the other session.
    3. With a `Withdraw(decimal amount)` method implemented as seen above!
    ```csharp
    public class BankAccount
        {
            private decimal balance;

            public decimal Balance => this.balance;

            public void PutMoney(decimal amount)
            {
                this.balance += amount;
            }

            public void Withdraw(decimal amount)
            {
                this.balance -= amount;
            }
        }
    ```
2. Right-click the Solution -> Add -> Solution Folder and call it Tests.
3. Add a new Unit Testing project inside the Tests folder. Usually Unit Test projects have the same name as the project they will cover with a ".Tetst" or ".UnitTests" suffix. In our case we want to create a Unit Test project "MyClasses.Tests" for the Library project we just created.
    1. In order to have access to the BankAccount class to test it, we need to add a reference in our Unit Test project to the "MyClasses" Library Project.

        ![add-reference-to-project-context-menu.png]({{"/assets/vs/add-reference-to-project-context-menu.png"}})

        ![add-reference-to-project-dialog.png]({{"/assets/vs/add-reference-to-project-dialog.png"}})
    2. Rename the File and Class "UnitTest1" created by default to "BankAccountTests"
    3. Create the Test Method for the happy path of the `Withdraw()` method
    4. Create the Test Method to ensure that it is not possible to withdraw more than we have in the account

Both Unit Tests would look as follows:

```csharp
[TestMethod]
public void CanWithdrawTest()
{
    const decimal initialBalance = 1000;
    const decimal toWithdraw = 500;
    decimal expectedFinalBalance = initialBalance - toWithdraw;

    var account = new BankAccount();
    account.PutMoney(initialBalance);
    account.Withdraw(toWithdraw);

    Assert.AreEqual(account.Balance, expectedFinalBalance);
}

[TestMethod]
[ExpectedException(typeof(ArgumentException))]
public void ExceptionThrownWhenAttemptingToWithdrawMoreThanAvailableTest()
{
    const decimal initialBalance = 1000;
    const decimal toWithdraw = 1500;
    var account = new BankAccount();

    account.PutMoney(initialBalance);
    account.Withdraw(toWithdraw);

    // assert is handled by the ExpectedException attribute
}
```

Notice the following things:
1. Test Methods have meaningful names and end with Test (this is one of several common naming ways out there).
2. The `[TestClass], `[TestMethod]' and `Assert` are part of Microsoft's Unit Testing Framework.
3. `Assert` object provides us with a handful set of methods to check the result of our Unit Tests
4. In our second Unit Test we are saying that the `BankAccount.Withdraw()` method should throw an `ArgumentException` when trying to get more money than available. This is not happening in our current implementation, therefore, the test will fail.

Let's run them. The easiest is to right-click the code inside a Test Class and click Run Tests (you will see the Test Explorer opening. You should see this as the result:

![test-explorer.png]({{"/assets/vs/test-explorer.png"}})

# Exercises
1. Improve the implementation of the `BankAccountWithdraw()` method to make sure it throws the expected exception when trying to get more money than available in the account and make sure the `ExceptionThrownWhenAttemptingToWithdrawMoreThanAvailableTest()` passes.
2. Extend the `BankAccount` class to have a withdrawal limit of 2000 (which is common in real life) and refactor the `Withdraw()` method to make sure this limit is taken into account. Throw an `InvalidOperationException`. We could use the same Exception as when exceeding the Balance, but in this way we will clearly see that the exception is thrown because of exceeding the limit.
    1. The `CanWithdrawTest()` should keep passing as we are using an amount lower than the limit
    2. In this test use an initial balance higher than the limit to make sure you unit test the limit and not the balance.
3. Unit Test the PutMoney function. How many Test Cases can you think about?

**Note**: in the [code I provide](https://github.com/nereolopez/csharp-intro) the `BankAccount` class has three Withdraw methods to show the evolution of the code:
1. `Withdraw()` is the initial one
2. `WithdrawCheckingBalance()` is the second one that takes Balance into account
3. `WithdrawCheckingLimit()` is the last version of the Withdraw() method you should end up with.

In you case, you should end up with just one `Withdraw()` method that should look similar to my `WithdrawCheckingLimit()` method (the same applies for the Unit Tests methods, and the same is true for the `PutMoney()` method).