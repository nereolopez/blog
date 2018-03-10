---
layout: post
title:  "C# Control Flow"
date:   2018-01-31 07:00:00 +0100
categories: software development
---
You might have already noticed that, given an entry point for our application (the Main method in the Program class in our sample Console Application) we write lines of code that the computer runs one after the other, or, in other words, sequentially.

![Sequential Flow.PNG]({{"/assets/sequential-flow.png"}})

But, sometimes, we might want to execute things in a different order based on what is going on dynamically in our application.

<!--more-->

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- ControlFlow.cs: where all the theory described here is shown.
- ControlFlowExercises.cs: where all the exercises at the end of this post are solved.

## Conditional Flow
The Conditional Flow breaks that linear sequence of statements and forks it into one or multiple "timelines" based on the evaluation of a condition. Let's see the picture below for a better understanding:

![Conditional Flow.PNG]({{"/assets/conditional-flow.png"}})

To break the sequential flow based on a condition there are several ways we can use to achieve it.

### The If
#### If Statement
`if` statement allows us to break the sequential flow based on the evaluation of an expression. This is its signature:

```csharp
if (condition) {
    // statements
}
```

We only need the `if` keyword, the expression to evaluate within the parentheses, and the brackets inside of which we will write the code that needs to be run if the condition is met. 

```csharp
if (5 > 2){
    Console.WriteLine("Five is greater than 2");
}
```

#### Else Statement
If we want to run some piece of code if the condition is not met, we could write another `if` statement checking for the opposite, but that is not needed, we can just use the `else` keyword instead, which gives as a block of code where we can write the code we need in case the expression is evaluated to be false.

```csharp
if (condition) {
    // code if expression evaluates to true
} 
else {
    // code if expression evaluates to be false
}
```

#### Nested Ifs
Well, this is not the best idea at all. You should avoid nesting things as much as you can, but, you need to know that this is possible.

```csharp
if (condition) {

    // run some code

    if ( anotherCondition) { 
        // code if nested 'if' is true
    }
    else {
        // code if nested 'if' is false
    }
}
```

#### If Else If
Well, you thought nested ifs were it? There is still more. Sometimes might not be enough to just say *"if this is not true then I do this"*. Sometimes you might want to say *"if this is not true, is this other condition met too so that I can do this and that?"*. That is the purpose for the `If Else If`.

```csharp
if (firstCondition) {
    // do something if expression is true
}
else if (secondCondition) {
    // only if first expression is false and second one is true, then do something here
}
```

#### Logical Operators
Well, sometimes evaluating just one expression is not enough. Let's say you usually walk to work, take the bus if it rains, and take a taxi if it rains and you are late for the bus. In this case, taking a taxi implies that two conditions have to be met at the same time: it has to be raining, and you missed the bus.

```csharp
if (isRaining && isLate){
    // take a taxi
}
```

This is the list of the logical operators:
- **`&&`**: the double ampersand is the logical operator for the **AND**
- **`||`**: the dobule pipe is the logical operator for the **OR**
- **`!`**: the closing exclamation mark is the logical operator for the **NOT**

#### Conditional Operator
Conditional Operator uses the `?` and `:` operators. It is a simplified way of using the ìf` statement. It is also known as Ternary Operator, 

```csharp
value = (condition) ? trueExpression : falseExpression;
```

### The Switch
Switch statement works pretty much as an If statement logically speaking. It is more effective but has some limitations. Let's analyze the keywords used on a Switch:
- **`switch`**: it starts the switch block and is followed by a condition
- **`case`**: you can have as many cases inside a switch block as you need. Each case is followed by a value. If the condition on the `switch` matches a `case`'s value, the the code within that `case` will be run.
- **`break`**: it forces the execution to exit the `switch` statement.
- **`default`**: this is another `case`, but this one is only executed if none of the other cases defined matches the condition.

```csharp
switch (condition){
    case value1:
        // execute some code;
        break;
    case value2:
        // execute some code;
        break;
    default:
        // execute some code;
        break;
}
```

What if we need the same code to be run within several cases? Just pile the cases one of top of another and write the code to be executed on the code block of the case in the bottom.

```csharp
switch (condition){
    case value1:
    case value2:
    case value 3:
        // code to be executed on cases 1, 2 and 3;
        break;
    default:
        // code to execute if no case is match.
        break;     
}
```

## Loops
Sometimes we may need to break the sequencial flow to do the same operation(s) over and over again for a known/unknown number of times based on whatever condition.
![Loop Flow.PNG]({{"/assets/loop-flow.png"}})

### For Loop
The `for` loop is used to repeat a specific number of times the statements within its block. Specific does not mean that we know at coding time how many times we have to do it, it might be a dynamic value set at runtime.

```csharp
for (initialValue; untilCondition; iterationStep){
    // statements to be executed
}
```

### Foreach
For now, we will leave the `foreach` aside. It works along with collections, and we'll come back to it once we reach Collections. 

### While Loop
The while loop is used to repeat the statements within its block as long as its condition is fulfilled. This means that, when the condition is not met any longer, the program will exit the loop.

```csharp
while (condition) {
    // statements to be executed;
}
```

## Exercises
1. Extend the "Profile" from the previous session
    1. Practice the `if`. Instead of displaying the "gender" with "f" or "m", convert it to "female" or "male.
    2. Practice the `switch`. Right after "years of experience", show the "seniority" of the user: < 2 years is Junior, 2 - 5 is Pro, 5-10 is Senior, 10+ is Manager. Use an `enum` **(note that in C# 7 is possible to use ranges in the `switch` `case`s)**.
    3. Choose the right loop strategy. Ask the user if the information is correct. It has to understand "yes" and "no" despite its casing. If the information is not correct, clear the Console and gather everything again.
2. Create a loop that generates 100 random numbers (between 0 and 100) and display how many of them were even and how many of them were odd.
3. Create a loop that generates random numbers (between 0 and 10) until the sum of all of them is greater than 100. Display each of them and the total of their sum.