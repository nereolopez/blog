---
layout: post
title:  "C# Variables and Types"
date:   2018-01-30 08:00:00 +0100
categories: software development
---
*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- Types.cs: where all the theory described here is shown.
- TypesExercises.cs: where all the exercises at the end of this post are solved.

## Introduction

Before beginning, let's see what `Types` are and which ones we can find built-in in .Net. 

Programming, information is classified as in real life. When you ask someone her phone number, you don't assume it is text, or that it is a date; in your mind, you see numbers. In the same way, when they ask you your name when you arrive to a hotel, the employee registering the check-in automatically places her hands on the keyboard side where letters are placed, and not in the numeric pad.

Same applies for programming, when we want to work with some data or information, we distinguish of which Type it is.

To be able to work with information that we will need throught our application, we need to store some of it on temporary memory slots in order to work with them, be it for checking it, to operate with it, to transform it, and so on. Variables are the slots where we will place this in-memory information.

Variables have to be typed (this means that we need to specify which type they will represent). There are two kind of types: `Value Types` and `Reference Types`.

## Value Types
Variables that are based on Value Types directly contain values. These are the Value Types built-in inside .Net:

- **bool**: this is a logical Value Type. Either it is `true` or `false`.
- **byte**:  a byte can be assigned a decimal literal, hexadecimal literal or binary literal that falls within its min value (0) and max value (255).
- **char**: it is used to represent a single character.
- **decimal**: of the floating-point types, is the one with more precission and smaller range (128-bit). We will mainly use it for financial and money calculations.
- **double**: is the 64-bit floating-point data type that is assigned by default if not specified otherwise when writing a real numeric literal on an assignment. 
- **enum**: defines an enumeration consisting on a set of named constants.
- **float**: represents a 32-bit floating-point value.
- **int**: it is a signed integer  of 32-bits. Its range goes from -2,147,483,648 to 2,147,483,647.
- **long**: it is another signed integer, in this case of 64-bits, and its range goes from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.
- **sbyte**: it is a signed 8-bit integer that goes from -128 to 127.
- **short**: it is a signed 16-bit integer that goes from -32,768 to 32,767.
- **struct**: it is a type used to encapsulate a related group of variables 
- **uint**: it is an **unsigned** 32-bit integer. Its range goes from 0 to 4,294,967,295.
- **ulong**: it is an **unsigned** 64-bit integer. Its range goes from 0 to 18,446,744,073,709,551,615.
- **ushort**: it is an **unsigned** 16-bit integer. Its range goes from 0 to 65,535.

## Reference Types
We mentioned before that Value Types directly contain data. Reference Types, on the other hand, store references to their data (objects). This means that, with Reference Types, two variables can reference the same object, meaning that operations from one variable will affect the object referenced by the other variable.

We will mention all of them so far, but only explain two that are interesting for us right now:

- class
- delegate
- dynamic
- interface
- **object**: this is a bit of special. All types, both predefined and user-defined, either reference or value types, inherit from 'Object'. We can assign values of any type to it. When a variable of Value Type is converted to it it is said to be `boxed`. And when it happens the other way around it is said to be `unboxed`.
- **string**: it represents a sequience of characters. Short said: text.

## Variables
We already discussed a bit before about variables and described them as the in-memory slots where we will place temporary information. We also mentioned that they have to be typed, so let's discuss them a bit ore in-depth.

### Variables declaration
C# needs you to declare a variable before being able to use it. To declare a variable, we just need to say which type of data it will contain, and then give it a name. 

*Note that every statement in C# ends with a semicolon `;`.*

```csharp
int i;
```
What we just did here was to declare an integer variable named *i*. 

*Good naming is very important to help others (and ourselves) understand the code. `i`might not be that meaningful if we are trying to store someone's age, `age` would be a more conveniant name for that variable in that case.*

### Variable Assignment
Once declared then we can use them. To assign a value to a variable we use the `=` operator and then the value on the right side of the expression.
```csharp
int age;
age = 20;
```

Declaration and value assignment can happen at once if we already know the value. It is perfectly valid to do something like this:

```csharp
int age = 20;
```

### bool Variables
`bool` variables can contain, as said, either `true` or `false`. This means that they are great to use for evaluating expressions and work with conditions. 

```csharp
bool isRaining= true;
```

### char Variables
They can contain a character. To assign a character value we need to use single quotes `'`.

```csharp
char firstLetter = 'a';
```

#### Char methods
Something not mentioned so far (and that applies to the other types too) is that `char` is an alias of `Char` ( in the same way that `bool`is an alias of `Boolean`, and so on).

`Char` object has some useful methods that are listed below:
- CompareTo()
- Equals()
- GetNumericValue()
- IsControl()
- IsDigit()
- IsLetter()
- IsLower()
- IsNumber()
- IsPunctuation()
- IsSeparator()
- IsSymbol()
- IsWhiteSpace()
- Parse()
- ToLower()
- ToString()

### int Variables
As said, it represents a signed integer, and we even had an example for it before with the age. It is possible to convert other types into `int`. we can just put `(int)` before the other value, or use the `Convert.ToInt32()` function. Keep in 

```
temperature = (int)25.5;      // in this case the decimal part will be lost, therefore, temperature will be 25.
temperature = Convert.ToInt32("25") ; // we are converting a string into an integer. 
```
### string Variables
We've already seen that we can shortly explain strings as text. Therefore, we could have a string variable as follows, just assigning a literal with double within quotes `" "` (note that we could also use the String constructor, which we will skip for now):
```csharp
string howToTriggerPeople = "Pineapple pizza is the best one";
```

#### string Operators
We can work with strings using methods or operators. For instance, we can compare two strings with the comparison equal `==` or distinct `!=` operators.

```csharp
string a = "hello";
string b = "Hello";

if (a == b) ....
```

The `+` operator can be used to concatenate (add) more text to our variable:

```csharp
string greeting = "Hello" + " Everyone!";
```

And the `[]` operator can be used to get a specific chacarter in our string:
```csharp
string greeting = "hello";
char e = greeting[1];
```

#### null and empty strings
When a string is declared but not given a value, its value is `null`. This is different from an empty string wich is represented as `""`. 
To check if a string is `null` we can just use the `==` operator against the `null` keyword.

```csharp
string str;
if (str == null)...
```
For checking an empty string we will see it now while reviewing String's methods.
 
#### String Fields and Properties
- **Emtpy** (field): Represents the empty string. We could compare against it to check if a string is empty.
- **Length** (property): Returns the length of the string (how many characters it has).

#### String Methods
We will review just a few, for the whole reference, check [here](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=netframework-4.7.1#Methods)

- **Concat()**: concatenates two given strings.
- **Contains()**: returns whether the string contains a given substring.
- **EndsWith()**: returns whether the string ends with a give substring.
- **IndexOf()**: returns the position within the string of a given character or substring.
- **IsNullOrEmpty()**: returns whether the string is null or empty.
- **IsNullOrWhiteSpace()**: returns whether the string is null or a white space.
- **Replace()**: returns a new string in which all occurrences of the given parameter are replace with it.
- **Split()**: splits the string into substring based on the given parameter.
- **StartsWith()**: returns whether the string starts with the given substring.
- **Substring()**: retrieves the substring of the string from the given position.
- **ToLower()**: converts the string to all lower case.
- **ToString()**: returns the instance of a String.
- **ToUpper()**: converts the string to all upper case.
- **Trim()**: removes all white spaces in the string (or a given characther if passed).

#### Immutability and StringBuilder
A `String` object is immutable, meaning that its value cannot be modified once it is created. Therefore, all methods that seem to modify it, are actually returning a `String` object containing the modifications.

This means that if we perform too many operations on a string, we are lowering performance. For cases when we need to do several operations on a string we should use the `StringBuilder` class instead.

```csharp
StringBuilder sentence = new StringBuilder();
sentence.Append("Hello");
sentence.Append(" World");
sentence.Length;      // returns 11
sentence.ToString();  // converts it to a string 
```

## DateTime
Even though `DateTime` is a `Struct`, let's see it separately. Again, we can directly assing it a value or use its constsructor. Let's assign our `DateTime` variable a value using one of the struct's properties:

```csharp
DateTime now = DateTime.Now;
```

### DateTime Properties
Let's review some of the DateTime properties. You can see the full list [here](https://docs.microsoft.com/en-us/dotnet/api/system.datetime?view=netframework-4.7.1#Properties).

- **Date**: gets the date of the instance.
- **Day**: gets the day of the month instance.
- **DayOfWeek**: gets the day of the week of the instance.
- **DayOfYear**: gets the day of the year of the instance.
- **Hour**: gets the hour of the instance.
- **Millilsecond**: gets the miliseconds of the instance.
- **Minute**: gets the minute component of the instance
- **Month**: gets the month of the instance.
- **Second**: gets the second of the instance.
- **Today**: gets the current date.
- **Year**: gets the year of the instance.

### DateTime Methods
Let's review some of the DateTime methods. You can see the full list [here](https://docs.microsoft.com/en-us/dotnet/api/system.datetime?view=netframework-4.7.1#Methods).

- Add()
- AddDays()
- AddHours()
- AddMinutes()
- AddMonths()
- AddSeconds()
- AddYears()
- IsDaylightSavingTime()
- IsLeapYear()
- Parse()
- Substract()
- ToLongDateString()
- ToLongTimeString()
- ToShortDateString()
- ToShortTimeString()
- ToString()

### DateTime Operators
Let's review some of the DateTime operators. You can see the full list [here](https://docs.microsoft.com/en-us/dotnet/api/system.datetime?view=netframework-4.7.1#Operators).

- Addition
- GreaterThan
- GreaterThanOrEqual
- LessThan
- LessThanOrEqual
- Substraction

## Exercises
These exercises are meant to be done in a Console Application for simplicity. As a hint, whenever you find yourself needing to use a method, just pay attention to the intellisense. It will tell you the different possible overloads for the method (we'll come to overloads later in these series) so that you can choose the one that interests you the most.

We will build a "user profile" only using variables and the I/O through the Console. For that:
1. Ask the user her name and store it in a variable.
2. Ask the user last name and store it in a variable.
3. Ask the user her birthdate and store it in a variable.
4. Ask the user to introduce her gender: 'F' for femle or 'M' for male.
5. Ask the user her profession and store it in a variable.
6. Ask the user her number of years of experience and store it in a variable.
7. Ask the user her gross yearly income and store it in a variable.
8. Show the user the following data as her information:
    - Name: {the name entered by the user}
    - Last Name: {last name entered by the user.}
    - Full Name: {LAST NAME, Name}
    - Birthdate: {birthdate entered by the user}
    - Age: {calculate her age and display it. Don't need to be accurate yet (if based on the month has already one more year)}
    - Gender: {gender entered by the user}
    - Profession: {show the profession entered by the user replacing white spaces with underscores _ }
    - Years of Experience: {number of years entered by the user}
    - Monthly Gross Salary: {salary entered by the user transformed into monthly}.