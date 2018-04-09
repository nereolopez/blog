---
layout: post
title:  "Selenium XPath"
date:   2018-04-09 07:00:00 +0100
categories: testing
---
XPath is a way to go down the webpage hierarchy to find a specific element. Let's see the basics of XPath to understand it, and also some tools to help us getting it right. Here is the most basic XPath syntax:

```
//tagname[@attribute='value']
```
- **//**: selects current node
- **tagname**: tagname of the node (input, div, etc)
- **@**: selects the attribute
- **attribute**: attribute to locate the element by
- **value**: value of the attribute we look for

<!--more-->
*Note that this post belongs to the [Automation Intro Series]({{ site.baseurl }}{% post_url 2018-03-26-automation-intro-series %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/selenium-intro).* 

## XPath Types

### Absolute XPath
Absolute Path allows you to go down the document since the beginning. For example, the website in the image below

![xpath-ui.png]({{"/assets/testing/xpath-ui.png"}})

has the following DOM structure to get to the second house in the list

![xpath-dom.png]({{"/assets/testing/xpath-dom.png"}})

This is how its absolute path would look like:
```
/html/body/div[2]/div[3]/div[2]
```
The code above is the same as saying:
1. Go inside the HTML tag
2. Go inside the Body tag
3. Go inside of the second div (the one with `class` *content*)
4. Go inside of the third div (the one with `id` *propertiesList*)
5. The home you were looking for is represented by the second div in the current level.

### Relative XPath
When using relative, we basically start looking at the point in the path that is more convenient for us, without the need to start from the root of the document. It starts with `//`.

Therefore, the path shown above, written as relative, would be as follows:
```
//div[@id='card-3']
```

In this case it is easy, because the card itself has an `id`. But, how would we do if it had not an `id`? As we don't want to start from the root, let's look for something we can easily grab. The next node up in the hierarchy is the `div` with the `id` *propertiesList*, so let's grab the second home in the list relatively from that node.
```
//html//div[@id='propertiesList']/div[2]
```

## Helper Tools
There are some tools that we can use to help us build the right XPath. I list some of them here:
- For Firefox:
    - [Firebug](https://getfirebug.com/)
    - [Firepath](https://addons.mozilla.org/en-US/firefox/addon/firepath/)
- Chrome
    - While inspecting using the Chrome Developer tools, you can right-click an element -> Copy -> Copy XPath
    - [ChroPath](https://chrome.google.com/webstore/detail/chropath/ljngjbnaijcbncmcnjfhigebomdlkcjo?hl=en)

## Exercises
Using the [Property Portal sample web]() from my wife (remember to go to the home page, which populates the local storage in order to have data), do the following **(do not use helper tools!)**:
1. Locate all elements using Absolute XPath
    1. Order by *los más nuevos*
    2. Select the second home (located in Avenida Carmilla)
    3. Assert it has 6 pictures.
    4. Leave a comment
    5. Assert number of likes has been incremented
2. Locate all elements using Relative Path 
    1. Order by *los que tienen más habitaciones*
    2. Select the third home (located in Calle Olentzero)
    3. Assert it has 4 pictures.
    4. Assert it has 6 bedrooms.
