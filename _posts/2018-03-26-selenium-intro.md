---
layout: post
title:  "Selenium Introduction"
date:   2018-03-26 07:00:00 +0100
categories: testing
---
Selenium automates browsers. This means that we can make browsers do things from code. Selenium offers a lot of different tools. In our case, we will focus on it's web driver, which supports different languages. To be able to use it with C# we need to add the needed packages to our project as it is shown in the next section.

<!--more-->
*Note that this post belongs to the [Automation Intro Series]({{ site.baseurl }}{% post_url 2018-03-26-automation-intro-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/selenium-intro).* 

## Setup on VS
1. Create a new **Unit Test Project** in Visual Studio.
2. Right-click the newly created project and click on **Manage NuGet Packages**.
3. Browse for the following packages and install them:
    1. Selenium.WebDriver
    2. Selenium.Support
    3. Selenium.WebDriver.ChromeDriver
    4. Selenium.WebDriver.IEDriver
    5. Selenium.Firefox.WebDriver
<img src="https://docs.microsoft.com/en-us/vsts/build-release/test/_img/continuous-test-selenium/continuous-test-selenium-02.png" alt="Adding the browser driver packages to your solution"/>

## First Selenium Test
Our project is now ready to start automating with Selenium and C#. This is how we could do very basic things:

```csharp
IWebDriver driver;

[TestMethod]
public void MyFirstTest()
{
    this.driver = new ChromeDriver();
    driver.Manage().Window.Maximize();
    driver.Manage().Timeouts().ImplicitlyWait(TimeSpan.FromSeconds(10));
    driver.Navigate().GoToUrl("http://google.com");
    this.driver.Quit();
}
```

Note that the method is decorated with the `[TestMethod]` attribute. Whenever we build our project, Visual Studio will add to the Test Explorer pane all the methods it finds with this attribute. If you are not familiar with the Test Explorer, you can review the [Unit Test series]() (not yet available) were we talked about it.

## Initializing a Test
In the same way as with Unit Tests, we can create a method that that will be run before the tests in the current class in order to initialize whatever we might need. In this case, we need to decorate the initialization method with the `[TestInitialize]` attribute.

```csharp
[TestInitialize]
public void Initialize()
{
    this.driver = new ChromeDriver();
    driver.Manage().Window.Maximize();
    driver.Manage().Timeouts().ImplicitlyWait(TimeSpan.FromSeconds(10));
}
```

Imagine all of the tests in our class will be run using Chrome and with the Chrome window maximized, then, it is better to set it up just once in the `TestInitialize` instead of doing it test by test.

## Test Cleanup
In the same way we can run a method before all of the tests in a class, we can run a method after our test. For doing it, we need to decorate that method with the `[TestCleanup]` attribute.

```csharp
[TestCleanup]
public void Cleanup()
{
    this.driver.Quit();
}
```

## Exercise
1. Create a new project in your solution for automating
2. Add the needed packages
3. Create the Initialize and Cleanup methods. Make sure to use the FirefoxDriver this time.
4. Create a first test opening your favourite site.