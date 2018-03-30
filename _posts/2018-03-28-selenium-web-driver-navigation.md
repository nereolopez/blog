---
layout: post
title:  "Selenium WebDriver and Navigation"
date:   2018-03-28 07:00:00 +0100
categories: testing
---
The `IWebDriver` can have several implementations. As you might have guessed already, `ChromeDriver` is its implementation to work with Google Chrome.

This interface defines methods and attributes that will help us interact with the browser by sending orders and getting information back.

<!--more-->
*Note that this post belongs to the [Automation Intro Series]({{ site.baseurl }}{% post_url 2018-03-26-automation-intro-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/selenium-intro).* 

## Web Driver

### Properties
Here are some of its common properties:
- **PageSource**: gets the source of the page last loaded by the browser.
- **Title**: gets the title of the current page.
- **Url**: loads a new web page in the current browser's window

### Methods
Here are some of its common methods:
- **Close()**: closes the current window, quitting the browser if it was its last window.
- **FindElement()**: gets an element based on the given `By` strategy.
- **FindElements()**: gets elements based on the given `By` strategy.
- **Navigate()**: we will see it in the next section below.
- **Quit()**: 
- **SwitchTo()**:

## Navigation
The `IWebDriver` has a `Navigate()` method, to which we will usually chain one of the following methods:
- **GoToUrl()**: navigates to the given URL.
- **Back()**: navigates to the previous page in the browser history.
- **Forward()**: navigates to the next page in the browser history.
- **Refresh()**: refreshes the page (same as pressing F5).

```csharp
[TestMethod]
public void NavigateToNavigateBackNavigateForwardRefreshTest()
{
    this.driver.Navigate().GoToUrl("https://google.com");
    Assert.AreEqual(this.driver.Title, "Google");

    this.driver.Navigate().GoToUrl("https://bing.com");
    Assert.AreEqual(this.driver.Title, "Bing");

    this.driver.Navigate().Back();
    Assert.AreEqual(this.driver.Title, "Google");

    this.driver.Navigate().Forward();
    Assert.AreEqual(this.driver.Title, "Bing");

    this.driver.Navigate().Refresh();
    Assert.AreEqual(this.driver.Title, "Bing");
}
```

## Exercises
Create an automation test achieving the following
1. Create a new Test Class with the Initialize and Cleanup methods, and create a test method too.
2. Open a new browser
3. Go to Google home page
4. Click on the *I'm Feeling Lucky* button
5. Store into a variable the page Title
6. Go back to Google home page
7. Check that the current page Title is different than the one you stored in a variable
8. Check that the current page URL against the one you used to navigate in step 3.