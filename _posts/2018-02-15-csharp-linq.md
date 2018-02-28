---
layout: post
title:  "C# LINQ"
date:   2018-02-15 07:00:00 +0100
categories: software development
---
# LINQ

What is it? It stands for Language INtegrated Query. Its name is pretty descriptive, so, as you migh have guessed already, it gives us the capacity to use Queries within the language itself, in this case, C#.

There are three parts for a LINQ operation:
1. Obtaining the Data Source.
2. Creating the Query.
3. Executing the Query.

<IMG src="https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/media/linq_query.png" alt="Complete LINQ Query Operation"/>

*Image from the Microsoft's Official [Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries#three-parts-of-a-query-operation)*

## LINQ Operation Parts

### The Data Source
Everything that supports `IEnumerable` or `IEnumerable<T>` or derives from it (like `IQueryable<T>`) can be handled by LINQ with no special treatment.  For example, we can have an array of numbers, which we've seen already.

```csharp
var numbers = new int[] {1, 2, 3, 4, 5};
```
An array is a valid example of a Data Source for LINQ.

### The Query
It specifies what information to retrieve from the Data Source(s). We can also use the Query to specify how that information should be grouped, or sorted before returning it. To create a Query we create a query expression and assign it to a variable to hold the Query.  If you are familiar with SQL it will be easy for you, if not, it will be easy too! ^^

Some keywords we will see in queries are `from`, `where` and `select`.

```csharp
var query = from num in numbers where (num % 2 == 0) select num;
```

### Query Execution
The Execution does not happen until we force the iteration over the source, because, in order to get the results, the Query does have to iterate the source. This means that, usually, until we do a `foreach` for example, the Query, even it was created, was not executed. 

```csharp
foreach(int number in query){ // only at this point is when the execution of the query happens
    Console.WriteLine(number); // would only display 2 and 4
}
```

It is also possible to force the execution of the Query by forcint the iteration over the Source in the Query declaration, Like the following example shows.

```csharp
var evenNumbers = (from num in numbers where (num % 2 == 0) select num).ToList();
```

## Basic Operations

### Filtering
This is probably the most common operation. We saw it in the example above filtering only the even numbers using the `where` keyword. Most of the times it's through a boolean expression that will include or exclude a specific element.

Filtering can use logical operators as any boolean expression:
- **`&&`**: logical AND.
- **`||`**: logical OR.
- **`!`**: logical NOT. 

```csharp
where (number % 2 == 0) && (number > 10) 
```

### Ordering
Usually we will want to return the result of a query ordered by a specific Property. Do you remember what the `T` in `List<T`>` for example stands for? Yes, it represents the type. [We will see later how to create Custom Types]() (not yet available), but so far, let's imagine that instead having a list of integers we have a list of Users, being User a custom type that has some properties like name or age.

We will need the `orderby` keyword and we can use the `ascending` or `descending`as needed.

```csharp
// users is a List<User>();
from user in users
where user.Age > 30
orderby name ascending
select user;
```

Keep in mind that if we want to order by more than one field, we could use the `ThenBy clause. 

### Grouping
We can use the keyword `group`. to get the results grouped `by` a key. For example, we want to group the users by their country of origin. The result of executing a query that groups is like a List of Lists. For example, if I have users from US, Canada and Spain, the result of executing a grouping by Country would give me a List with three Lists inside (one for the users from US, one for the users from Canada and another one for the users from Spain). Each of these inner Lists, additionally to having the users as elements, they have also a `Key`. As we grouped by Country, the `Key` of each inner List will be the country they hold users from.

```csharp
// users is a List<User>();
var usersByCountry = from user in users group user by user.Country;

Console.WriteLine("Let's see the users per Country.");
foreach(var country in usersByCountry) {
    Console.WriteLine("These are the users from {0}", country.Key); // notice the country.Key! country is one of the inner Lists
    foreach(var person in country) {
        Console.WriteLine("- {0}, {1}, {2}", person.Name, person.Age, person.Country);
    }
}
```

### Joining
LINQ can have several Data Source types, but when Joniing is the exception, it **always** works against object collections. The `join` keyword allows us to create sequences that are not directly inside our data sources. For example, imagine that we want to get Users and Training Centers that are in the same location.

```csharp
var query = 
    from user in users  
    join center in centers on user.Country equals center.Country 
    select new { UserName = user.Name, TrainingCenterName = center.Name };

foreach(var userCenter in query)
{
    Console.WriteLine("User: {0}, Local Training Center: {1}", userCenter.UserName, userCenter.TrainingCenterName);
}
```

Notice the following keywords: `join`, `on` and `equals`. To get the local Training Centers for a user we will create a match using the `join` `on` those Users whose location `equals` a training center`s location. 

In the previous examples the `select` keyword was returning directly each of the elements inside the Data Source that match the criteria (for example, in the Ordering sample code we can read the `select users`). In this case, as we are joining, we probably want to return properties from the different entities we are crossing, that's when the las line `select new {  }` comes into the game. In the example above we are saying that we will return, for each match in the Data Source against the Query criteria, a new entity consisting of two properties (UserName and TrainingCenterName) that will take the values from the fields assigned from the entities used ni the query (the name of the User and the name of the Training center in this case).

### Select (Projections)
What we've seen with the `select` keyword is how we can shape what the query will return once executed. We saw that it is possible to return each matching result with the "shape" of an entity (like a User), we could say that it should return just a subset of an object's properties, or even say that it should return properties from different objects (as shown in the Joining example).

## Query Operator Methods
It is possible to use LINQ as if we were using methods over Collections. For instance, if we have a `List<T>` we could use the `.Sort()` method on it.

```csharp
users.OrderBy(user => user.Name).ThenBy(user => user.Country);
```

## Other Query Operators
There are many operators that can be used with LINQ. You can check the official documentation for the [full list of operators against Enumberables](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable?view=netframework-4.7.1), you can check the methods against Queryables [here](https://docs.microsoft.com/en-us/dotnet/api/system.linq.queryable?view=netframework-4.7.1).
- **All**
- **Any**
- **Average**
- **Contains**
- **Count**
- **Distinct**
- **Empty**
- **First**
- **FirstOrDefault**
- **Last**
- **LastOrDefault**
- **Max**
- **Min**
- **Range**
- **Repeat**
- **Single**
- **SingleOrDefault**
- **Skip**
- **SkiptLast**
- **SkipWhile**
- **Sum**
- **Take**
- **TakeLast**
- **TakeWhile**

# Exercises

1. Queries Exercises:
    1. Create a `class` Month with three Properties: Name, Season and Number of Days 
    2. Fill in a `List<Month>` with the list of the months already set up.
    3. Show on screen all the months that contain an "o" (without being case sensitive).
    4. Show on screen the months grouped by season.
2. Operator Methods
    1. Show on screen all the months skipping those with odd number of days.
    2. Reuse the `Array` of Students and Grades from the [Array Class Exercises](https://nereolopezblog.wordpress.com/2018/02/06/c-arrays/) and (don't do it calculating them by hand as in the Arrays class, but using some of the Query Operators we just reviewed):
        1. Print the Average of each Student.
        2. Show the Higher and the Lower averages