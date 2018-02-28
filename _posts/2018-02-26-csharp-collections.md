---
layout: post
title:  "C# Collections"
date:   2018-02-26 07:00:00 +0100
categories: software development
---
Let's think of Collections as a group of related objects. So far we've seen arrays, which are useful to work with a fixed number of strongly-typed objects.

Collections' size is dynamic, meaning that collections can grow or shrink at any time. Some kind of collections even allows us to assign a key to an object for easier retrieval. 

Keep in mind that a `Collection` is a `Class`, which means that it has to be instantiated using the `new` keyword. But [we will see more on classes later]() (not yet available).

# Overview

## Common Properties and Methods
Let's not forget that there can be several representations for a collection. Each of them implements the `ICollection<T>` interface, which grants that they will have some common Properties and Methods. Don't worry to much about interfaces now, as [we will see them later]()(not yet available).

- **Count**: gets the number of elements contained in the list.
- **Add()**: adds the given element to the collection.
- **Clear()**; removes all objects from the collection.
- **Remove()**: removes a given element from the collection.

*Note that not all the methos here might exist ini all the collection representations. Like `Add()` or `Remove()` which are not present in a `Stack` or in a `Queue` collection. We will see one by one.*

## Instantiating a Collection
We will use the `List<T>` for our generic samples.  note that the `<T>` means that the `List` class is strongly typed, and that we need to specify of which type the elements of the list will be.

```csharp
var names = new List<string>();
```

To instantiate an object we usually use the `new` keyword followed by the name of the object we want to instantiate plus the parentheses `()`. Note that in this case we have the `<T>` between the name of the class to be instantiated and the `()` that represent the call to the class' constructor.

## Adding Elements
To add an element we just use corresponsing method. Note that when we are using strongly-typed collection types, the Intellisense will already tell us of which type the element to add should be.

```csharp
var names = new List<string>();
names.Add("Larry");
```

## Iterating over a Collection
It works the same way we did with arrays, we can use any kind of loop that keeps an index (ike the `for` loop) and access the current element with the `[]` operator, or use a `foreach` loop that gives us directly the current element.  Let's see an example using the list of names from above.

```csharp
foreach(var name in names){
    Console.WriteLine(name);
}
``` 

## Removing Elements from a Collection
*Keep in mind that we are taking a look at generic examples using a `List`, we will see later how to do it on different Collection types.*
We can remove an item from the collection by using the `Remove()` method, wich expects the item to be removed as an argument.

```csharp
names.Remove("Larry");
``` 

# List<T>
We already saw most of the `List` basics in the overview, but let's cover two more things: Its Capacity and how to Insert elements.

## Insert
We said that the `Add()` method puts another element inside the collection. Why do we need an `Insert()` method then? Well, the difference is that the `Add()` always puts the given element inside the collection as the last element of it. On the other hand, the `Insert()` alows you to specify in which position you want to put it.

```csharp
var cars = new List<string>();
cars.Add("Citroen C1");
cars.Add("Toyota CHR");
cars.Add("Mercedes S Coupe");

cars.Insert(2, "Volvo XC 60"); // will add it right after the Toyota CHR, being the Mercedes still the last element
```

## Capacity
The `Capacity` property is an internal counter that tells us how many elements we can have without the need to resize the `List`. We can remove the gap between the actual number of elements and the total capacity using the `TrimExcess()` method. Let's contiune with the list from above about cars for this example

```csharp
cars.Insert(3, "Audi A6"); // adds the Audi A6 after the Volvo
Console.WriteLine(cars.Count); // will print 5 as we now have 5 cars
Console. WriteLine(cars.Capacity); // will print 8 as it is its current capacity without the need of resizing to host more elements
cars.TrimExcess(); // removes the excess of capacity
Console.WriteLine(cars.Capacity); // will now print 5 as the excess was removed
```

# Dictionary
Dictionaries are Collections that allows us to store information in a key/value format. This means that for every `Value` we add to the collection, we need to add a `Key` associated with it.

Each `key` is unique (and I say 'is' because a Dictionary won't allow you to enter a duplicated key). Retrieving values by its key is very fast. If we use an object as a key, it shall not change in any way that affects its hash value. And remember that keys cannot be null (though values can).

## Declaring a Dictionary
Let's create a Dictionary that will contain a student with its Id and its Name.

```csharp
var students = new Dictionary<int, string>();
```
Notice the following:
- As usual we create a variable and give it a name.
- Then we use the `new` keyword and the type we want our variable to be an instance of (`Dictionary` in this case).
- `<T>`. We explained this while speaking about the `List<T>`. This means that our type is strongly-typed. In the case of a `Dictionary`, as we will have a `Key` and a `Value` we need to specify both inside the `<>`. That is why in the example above we have `<int, string>`, being `int` the type for the Id of the student, and `string` the type for the Value of the student.
- Then `()` parentheses follow to complete the call to the constructor of the `Dictionary`.


## Dictionary Properties
Let's review some of the most used properties for a Dictionary:
- **Count**: gets the number of key/value pairs in the collection.
- **Item[TKey]**: gets or sets the value associated with the given key.
- **Keys**: gets a Collection with the keys of the Dictionary.
- **Values**: gets a Collection with the values of the Dictionary.

## Dictionary Methods
Let's review now some of the most common methods of a Dictionary (see the [official documentation](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=netframework-4.7.1#Methods) for the whole list).
- **Add(TKey, TValue)**: adds a new value with its key.
- **Clear()**: removes all keys and values from the dictionary.
- **Contains(TKey)**: determines whether the Dictionary contains the given Key.
- **ContainsValue(TValue)**: determines whether the Dictionary contains the given Value.
- **Remove(TKey)**: removes the Value and its Key from the Dictionary.
- **TryGetValue(TKey, TValue)**: gets the Value associated with the given Key.

# Queue
A `Queue` represents a first-in first-out (FIFO) colletion. This means that when we remove something from the collection, it is the oldest element the one that will be removed.

![image.png](.attachments/image-2c4519ce-0ab1-493f-b754-fc85280d9a05.png)

As depicted above, from the right we are adding numbers. When the 1 is added it goes to the beginning (first slot) of the Queue. When we remove something from the Queue, the element at the beginning of the Queue is the one removed and the others are moved one position to the beginning of the Queue. For example, if we remove one number from the Queue above, the 1 will be removed and the 2 will be then in the beginning of the Queue.

```csharp
var numbers = new Queue<int>();
```
We will mostly use Queues or Stacks when we need to store temporary information. Queues when we want to read the information in the same order it gets stored, and Stacks in reverse order.

## Queue Properties
- **Count**: gets the number of elements in the `Queue`.

## Queue Methods
Let's review some of the most common methods for a `Queue`.
- **Clear()**: removes all objects from the Queue.
- **Contains(T)**: determines whether an element is in the Queue.
- **Dequeue()**: removes and returns the element at the beginning of the Queue.
- **Enqueue(T)**: adds an object to the end of the Queue.
- **Peek()**: returns the object at the beginning of the Queue **without removing it**.
- **ToArray()**: copies the elements to an `array`.

# Stack
This is the opposite to a `Queue`, which means that is a last-in first-out (LIFO) collection.

![image.png](.attachments/image-872f1352-d403-4e63-8626-7f8298da76de.png)

As depicted above, the `Stack` can be thought as a pile of elements. Whenever we push something into it from the top, it falls directly to the bottom. As the only place from where we can add or remove anything is the top, the last element that entered into the `Stack` will be the first one removed.

```csharp
var numbers = new Stack<int>();
```

## Stack Properties
- **Count**: gets the number of elements in the `Stack`.

## Stack Methods
- **Clear()**: removes all the elements from the Stack.
- **Contains(T)**: determines whether an element is in the Stack.
- **Peek()**: returns the element at the top of the Stack **without removing it**.
- **Pop()**: removes and returns the element at the top of the Stack.
- **Push(T)**: inserts an element to the top of the Stack.
- **ToArray()**: copies the elements to an `array`.

#Exercises
1. Create a List of 50 numbers and fill it with 25 randomly generated numbers
    1. Do not add numbers that already exist.
    2. Sort the numbers in the List from lowest to highest.
    3. Insert the missing numbers in the sequence up to 50.
    4. Remove those that are not multiple of 3.
    5. Display the final List.
2. Create a small program to handle favourite sites' links where fom the options in the menu below. Note that after every action the user is sent back to the menu, which contains these options: 
    1. Add Site (has to provide site name and URL. Cannot add duplicate sites).
    2. View Sites (has to list all the sites with the name of the site and its url)
    3. Remove Site (has to list all the sites and ask her which one should be removed. If the one provided is not correct, should be handled and ask if she wants to try again or go back to the menu).
    4. Exit the application. 

*Note that for the last exercise it is not needed to persist the information anywhere, only in-memory information will be available for the user.*