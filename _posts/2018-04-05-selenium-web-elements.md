---
layout: post
title:  "Selenium - Interacting with Web Elements"
date:   2018-04-05 07:00:00 +0100
categories: testing
---
The `WebElement` object in Selenium represents a Web Element in the web page (a paragraph, a button... whatever), and gives us different properties and methods that we can use. We will review these methods and properties, as well as how to retrieve the elements we are interested in with different strategies.

<!--more-->
*Note that this post belongs to the [Automation Intro Series]({{ site.baseurl }}{% post_url 2018-03-26-automation-intro-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/selenium-intro).* 

## Web Element

### Properties
Let's see some of the properties it offers:
- **Displayed**: returns whether the element is being displayed or not. It is not related to whether it is in the DOM or not. 
- **Enabled**: returns wether the element is enabled or not.
- **Location**: returns a `Point` object with the location of the element.
- **Selected**: returns whether the element is selected or not.
- **Size**: returns the size of the element with its Weight and Height.
- **TagName**: returns the name of the tag of the element. For instance, for the element `<input name="user">` the TagName will return `input`.
- **Text**: returns the visible inner text of the element.

### Methods
Let's see some of the methods it offers:
- **Clear()**: if the element is an entry element, it will clear its content.
- **Click()**: clicks on the element.
- **GetAttribute()**: gets the value of the given attribute of an element. For example: `element.GetAttribute("id")` will return the value of the `id` attribute.
- **GetCssValue()**: gets the value of the element's CSS.
- **SendKeys()**: simulates writing into an element, sending the keys we specify.
- **Submit()**: similar to the `Click()`, but works better on elements that are part of a form.

## Finding Elements (Basics)
There are two methods that are clearly focused on getting an `IWebElement`: `FindElement()` and `FindElements()`. Let's see first what their differences are before studying how to use them.

**FindElement()**:
- If no element is found, throws a `NoSuchElementException`.
- If one element is found, returns the element.
- If more than one element is found, returns the first one.

**FindElements()**:
- If no element is found, returns an empty collection.
- If one element is found, returns a collection with the only found element.
- If more than one element is found, returns the collection with all the found elements.

### By
Both methods expect to be passed a `By`. It allows us to define the criteria by which we want to get an element. It could be the tag itself, the value of one of its attributes, or the path until the element within the DOM. We will see some of the different options right now:
- **ClassName()**: tries to find an element based on the given class name.
- **Id()**: tries to find an element based on the given id.
- **LinkText()**: tries to find an element based on the given text.
- **Name()**: tries to find an element based on the given name.
- **PartialLInkText()**: tries to find an element which part of its link text is the given string.
- **TagName()**: tries to get an element based on the given tag name.


## Select Class
When working with Dropdown or Multiselect elements, we can use the techniques mentioned above to locate an element of either these types, but, to interact with them, we need to use the `Select` class provided with the Selenium Support package (`OpenQA.Selenium.Support.UI` in this case). Let's take a look at the properties and methods it provides:

### Properties
Let's see some of the properties it offers:
- **AllSelectedOptions**: returns a list with all the selected option elements inside the Select element.
- **IsMultiple**: returns if the Select element supports multiple selection.
- **Options**: returns a list with all the option elements belonging to the Select element.
- **SelectedOption**: returns the selected option.

### Methods
Let's see some of the methods it offers:
- **DeselectAll()**: deselects all the options within a selectable element.
- **DeselectByIndex()**: deselects the option with the given index.
- **DeselectByText()**: deselects the option with the given text.
- **DeselectByValue()**: deselects the option with the given value.
- **SelectByIndex()**: selects the option with the given index.
- **SelectByText()**: selects the option with the given text.
- **SelectByValue()**: selects the option with the given value.

## Exercises
We will be using some free pages that are meant to practice automation.
1. For the first exercise, we will be using [this form from TheAutomatedTester](http://book.theautomatedtester.co.uk/chapter1)
    1. Check the available radio button
    2. With the Dropdown
        1. Assert that *Selenium IDE* is the default value.
        2. Switch the selected option to *Selenium Grid*.
    3. Click on the *Home Page* link and Assert that *Chapter 1* element is available.
    4. Navigate back.
    5. Check the Checkbox besides the Dropdown.
    6. Assert the *Assert that this text is on the page* is on the page.
2. Using the [Property Portal properties list page](https://grup5web.firebaseapp.com/properties/properties.html?region=Navarra) my wife did as a demo site with some team mates in the University, do the following:
    1. This sample app works with data initialized in the local storage, so you will have to navigate to the [home page](https://grup5web.firebaseapp.com/) first.
    2. Navigate to the Properties List Page and get Price, Number of Bedrooms and Interested people.
    3. Click on it to go to the details page and Assert the info is the same in the details view.
    4. Send a comment and check that the interested people has incremented by one.
    5. Navigate back to the properties list page.
    6. Check that the insterested people is incremented also in the list page for the selected property.