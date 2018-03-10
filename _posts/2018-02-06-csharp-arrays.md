---
layout: post
title:  "C# Arrays"
date:   2018-02-06 07:00:00 +0100
categories: software development
---
What is an array? An array is a collection or group of things of the same kind. For instance, the winning numbers of the lottery. They are all of the same type and have a relationship between them.

Arrays in C# follow the principles below:
- They can be single-dimensional, multidimensional or jagged.
- Array dimentions are set in the creation and cannot be modified later.
- The default value of numeric elements are defaulted to `0`, and reference elements are defaulted to `null`.
- Arrays' index start on 0.

<!--more-->

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Arrays.cs: where all the theory described here is shown.
- ArraysExercises.cs: where all the exercises at the end of this post are solved.

## Arrays Creation

### Array Declaration
As with any other type, to declare an array we start specifying its type. Well, we will not write `array` before the name of the variable as if it were a type. Instead, we will write the type of the elements it will contain. **Remember that all the elements within an array are of the same type**.
Additionally, right after the type (without any white space), we will use the `[]` operator to say that it is an array. When instantiating it we can also say between brackets the number of elements it will contain.

*Remember that its index starts in 0, so if we declare an array with a size of 10 elements, its poseessitions will go from 0 to 9.*

```csharp
int[] numbers = new int[10];
```

### Array Initialization
It is possible to initialize an array on its declaration. We do so by using curly brackets `{}` and placing the elements inside separated by comma `,`. In this case it is not necessary to specify how many elements it will contain, as it can be infered just considering the number of elements you initialized.

```csharp
string[] students = new string[] { "Lucy", "John", "Jessica", "Brian" };
```

It is also possible to assign the elements directly as a shortcut, like this:

```csharp
string[] seasons = { "Summer", "Fall", "Winter", "Spring" };
```

### Implicitly Typed Arrays
The same way it is possible to infer the number of elements an array will contain by checking how many elements are initialized, we can infer the type of the array by realizing the type of its declared elements.

```csharp
var numbers = new[] {1, 2, 3, 4, 5};
```

### Delayed Initialization
We can declare the variable without instantiating it, but then, when we want to initialize it, we need to use the `new` keyword.

```csharp
int[] numbers;
numbers = new int[]{ 1, 2, 3, 4, 5};
```

## Working with Arrays

### Setting Values
If we need to set the value of an array we can do that by just accessing to the position of the element we want to change (remember that its index starts in 0!!). To access a position within the array we use the `[]` with an index inside of it, then, we assign it as we would do with any other variable.

```csharp
string[] days = new string[7];
days[0] = "Monday";
```

### Getting Values
Is exactly the same as setting values, but without assigning anything. Just the name of the array, the brackets, and the index we want to access inside of the brackets.

```csharp
Console.WriteLine(days[0]); // prints 'Monday' if we follow the previous example.
```

## Array Properties and Methods

### Properties
- **IsFixedSize**: wait, what?? We said arrays are always of the same size once created. Why does it have a property to check that? Well, it is to be compliant with the `IList` interface the `Array` class implements ([more later on interfaces]() (not available yet)), but as you can imagine, this will always return `true`.
- **Length**: gets the total number of elements of the array (**considering all its dimentions!,** more a bit later in this article about array dimensions).

### Methods
- **Clear(array, start, end)**: sets the elements of the given array on the given range to their default value.
- **Clone()**: creates a copy of the copy of the array.
- **Exists(array, predicate)**:  returns if an element that matches the given predicate exists in the array.
- **Find(array, predicate)**: returns the first ocurrence in the array that matches the given predicate.
- **FinIndex(array, predicate)**: returns the index of the first occurrence in the array that matches the given predicate.
- **IndexOf(array, value)**: returns the index of the given value if present. If not, returns -1.
- **Reverse(array)**: changes the order of the array's elements based on their position inside of the array.
- **Sort(array)**: its default overload which only takes an array sorts it alphabetically.

*Note: take into account that some of the `Array` methods only work in single dimention arrays.* 

## Iterating Arrays
In the [Control Flow session](https://nereolopezblog.wordpress.com/2018/01/31/c-control-flow/) we learnt about loops. One way to iterate an array is using a `for` loop, but, as we are working now with a simple form of collections, let's introduce a new way to iterate, in this case, collections: `foreach`.

The `foreach` expects between the parentheses a variable that will represent the element of the array in the current iteration. This variable is declared inside the parentheses, and it is follow of the keyword `in`and the array to iterate through. 

*As a recommendation, try to give the variable a name as semantic as possible for the sake of having a readable code. You will see what I mean in the example below.* 

```csharp
// less semantic approach
foreach(int i in numbers) { }

// more readable manner
foreach(int number in numbers) { }
```

**Notice that the variable declared to represent the current element has to be of the same type of the array's elements.**

##Multidimensional Arrays
To declare it is almost the same as with the single dimension array, but when specifying its size we will have n elements, being n the number of dimentions it will have.

For example:

```csharp
var numbers = new int[4,4];
```

In this case we have 2 elements inside of the brackets, which means that this array has 2 dimensions, and each dimension, in this case, will have 4 elements. To access one element in a multidimensional array:

```csharp
numbers[0,0];
```

## Jagged Arrays
Jagged Arrays are nothing else than arrays inside arrays, yeap, you got it right, it's arrayception :D (you got it?.. never mind, tough public here ^^').
Let's quickly see them:

```csharp
var monthsBySeason = new string[][]
{
    new string[] { "December, January, February" },
    new string[] { "March", "April" , "May" },
    new string[] { "June" , "July" , "August" },
    new string[] { "September", "October", "November" }
};

Console.WriteLine("Let's see what month position 2, 1 returns: " + monthsBySeason[2][1]); // returns July
```
Notice that opposite to the Multidimensional Arrays, to access an element we used more than one pair of brackets, like `[2][1]` in the example above. 

## Exercises
1. Create an array with all the Months.
    1. Show on screen how many months are in the array.
    2. Show on screen all the months that contain an 'o'.
2. Create an array that represents 5 students and their grades in 5 subjects.
    1. Show the grades for each student.
    2. Show the grades for each student in desc order.
    3. Calculate the average of each of them and show the student (index of the student) with the highest average and his average, and the student with the lowest average and his average.

*Note that it is possible to solve them with more methods from Array, for example, to find the average, but the code provided is done so that we practice how to iterate over arrays and do it ourselves.*