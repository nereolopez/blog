---
layout: post
title:  C# TDD
date:   2018-03-23 08:00:00 +0100
categories: software development
---

Test Driven Development is, shortly said, a technique that consists in writing the tests before the actual code. Wait, what?? How am I supposed to call a method that does not exist to test it? Well, the method will exist, but will not have the new functionality we want to add, therefore, the test will fail. Then, we will write the new small code to make it pass.

<!--more-->

*Note that this post belongs to the [C# Unit Test Series]({{ site.baseurl }}{% post_url 2018-03-20-csharp-unit-test-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro). The relevant code for Unit Testing is in the folders that finish with the **.Tests** suffix. The classes they test are in their pair projects (for instance, the code that is tested in the `MyClasses.Tests` project is inside the `MyClasses` project)*.

Yes, I said small for one reason. This technique is based on very small iterations. Let's think of a function we did in the session about [Unit Tests Setup and Structure]({{ site.baseurl }}{% post_url 2018-03-22-csharp-unit-test-structure %}). There, we wrote a method called `Divide(double a, double b)` inside the `PureFunctions` class. 
Our first test case will be that the method returns the result. Therefore, if I pass it a 4 and a 2, it should give me back a 2. Clear. Then, we added a second test case, which was to avoid errors by trying to divide by 0. We did not use TDD, so let's see how it would have looked like if we had used it. We would have started with our actual code in the following state:

```csharp
public double Divide(double a, double b){
    return a / b;
}
```

Great. Now, applying TDD we will write the code for our new Test Case, which is that, if given a zero to divide by, the method should throw an exception.
```csharp
[TestMethod]
public void DivideThrowsExceptionWhenDividingByZeroTest()
{
    // Arrange
    var functions = new PureFunctions();

    // Act and Assert
    Assert.ThrowsException<ArgumentException>(() => functions.Divide(7, 0));
}
```

As you are thinking, this test will fail! That is ok. Why? When we write the code first we could end up in a situation where we write the tests to match the implementation we have, but, in this way, we are writing the tests against our Test Cases. Writing the test first, once it passes we know the functionality is really doing what it is expected. So, at this point, the only missing thing is to evolve our `Divide()` method to make sure we cover the scenario depicted in the test above:
```csharp
public double Divide(double a, double b){
    if (b == 0) throw new ArgumentException("Not possible to divide by 0");
    return a / b;
}
```

Keep in mind that this is not only for new functionality, but also when something has to be adapted or refactored. Additionally, you might find yourself refactoring your code to refine it as you are adding more test cases to  your functionality.

Let's stop talking here and let's practice a bit.

## Exercises
Create a `TestDriven` class in `MyClasses` project and the corresponding Test Class in the Test Project.
1. Let's grow our app (Car Rental App Part IV already!) by doing the closure of a Rental using TDD. You can [review the requirements for closure here](https://nereolopezblog.wordpress.com/2018/02/22/c-first-app-part-i/).
**We will first make sure all the structure (service, manager and program are adapted), think of the test cases and then implement it together** 
2. Fibonacci Sequence
    1. Passing a zero should return zero
    2. Passing a one should return one
    3. Passing a two should return one
    4. Passing a three should return two
    5. Passing a seven should return thirteen (all sequence should work by now)
