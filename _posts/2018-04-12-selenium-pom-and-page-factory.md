---
layout: post
title:  "Selenium Page Object Model & PageFactory"
date:   2018-04-12 06:00:00 +0100
categories: testing
---
Good practices and patterns are not only reserved for developers. Automation code can get huge too, and therefore, should be maintainable. 

So far we did everything inside test methods, but most likely, you don't want to do that in real life. Every application has a domain, and therefore, objects that represent it. The Page Object Model pursues the idea of creating objects that can be representative of one part of your domain, or of one page, allowing us to easily map our Model's properties with elements on the correponding page.

<!--more-->
*Note that this post belongs to the [Automation Intro Series]({{ site.baseurl }}{% post_url 2018-03-26-automation-intro-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/selenium-intro).* 

## Page Object Model
With POM we can achieve the following:
1. Separate Tests and Verification from UI flow and Operations
2. Separate the Object Repository from the Test Classes
3. We can apply good OOP practices (review the [Encapsulation](), [Inheritance]() and [Polymorphism]() articles if needed) 
4. Less code and more maintainability

Imagine we have a Login page with the user and password fields, and the login button. How do we translate this to POM? We just need to create a class that represents our page, the properties that will represent our web elements, and decorate these properties. Instead of grabbing them manually in the constructor or in any other method, we will use a built-in attribute that will scan the page the elements and assign them to the decorated properties. Somethin like this:

```csharp
public class Login
{
    [FindsBy (How = How.Id, Using = "username")]
    public IWebElement Username { get; set; } 

    [FindsBy (How = How.Id, Using = "password")]
    public IWebElement Password { get; set; }

    [FindsBy(How = How.Id, Using = "loginButton")]
    public IWebElement LoginButton { get; set; }
}
```

## PageFactory
`PageFactory` class has a static method that allows us to give it a class and it will find all the members decorated with the `FindsBy` attribute and will link that member to the matching `WebElement`. If we assume we are testing the Login we have above, in our test method it could look something like this:

```csharp
var login = new Login();
PageFactory.InitElements(this.driver, login);
```
Then, we could directly use the login attributes as now they are mapped to an element in our page.
```csharp
login.Username.SendKeys("LordDarkHelmet");
login.Password.SendKeys("123456");
login.LoginButton.Submit();
```

One good ooption would be to have the `PageFactory` initializing the object elements from within its `constructor`.

## Applying Best Practices
Of course, you may want to encapsulate some elements, or, in your domain representation, you might need some inheritance within your model, or whatever other best practice. Even it is testing code, feel free to apply any OOP practice or whatever Design Pattern you think might fit, not for the sake of doing it, but to have a clean and scalable code.

For instance, following with the Login example, let's encapsulate the elements:
```csharp
public class Login
{
    [FindsBy (How = How.Id, Using = "username")]
    private IWebElement Username { get; set; } 

    [FindsBy (How = How.Id, Using = "password")]
    private IWebElement Password { get; set; }

    [FindsBy(How = How.Id, Using = "loginButton")]
    private IWebElement LoginButton { get; set; }

    public Login(IWebDriver driver) {
        PageFactory.InitElements(driver, this);
    }

    public void SetCredentials(string username, string password) {
        this.Username.SendKeys(username);
        this.Password.SendKeys(password);
    }

    public void LogIntoApp() {
        this.LoginButton.Submit();
    }
}
```

With this new approach, our test becomes much more semantinc:
```csharp
var login = new Login(); // now the object initializes itself on instantiation!
login.SetCredentials("LordDarkHelmet", "123456");
login.LogIntoApp();

// whatever assert
```

## Exercises
As in the previous sessions, we will use the sample Property Portal web my wife did (remember to go to the home page the first time to make sure the site loads sample data in the local storage).

1. Go to the [property details page](https://grup5web.firebaseapp.com/property-details/property-details.html?id=9)
2. Use the POM and PageFactory for this page.
3. Navigate all the way down to the first property using the "Ir a la anterior vivienda" navigation link (you can know when you reach the first one by checking if this link is visible or not)
4.  Assert each navigation. You can make sure you changed to a different home by locating the "propertyId" hidden input (make sure it is part of your POM!)
5. Navigate all the way up to the last home
6. Assert that the information you had in the beginning for this home is the same as you haven now you navigated back