---
layout: post
title:  "C# File System"
date:   2018-03-07 07:00:00 +0100
categories: software development
---
*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is in the following files:*
- Program.cs: entry point for the sample Console Application
- FileSystemSamples.cs: where all the theory described here is shown.

## Introduction
There are several classes that come with the Framework that allow us to work with the File System:
- System.IO.FileInfo
- System.IO.DirectoryInfo
- System.IO.DriveInfo
- System.IO.Directory
- System.IO.File

For this introduction, we will mainly focus on the last two (Directory and File) which represent a file or directory, offer some of their properties, and also methods to open, close, move or delete files and folders.

## Initial Considerations
- Always check user specified path strings. They could be malformed, contain invalid characters or exceed the maximum allowed length.
- Always check if a given file name is `null`.
- Always check if a given file name is longer that allowed.
- Always check if a file name contains invalid characters.

*Keep in mind that if the application does not have sufficient permissions to access a file or folder, the `Exists()` method will return `false` even if it exists.*

## Directory Class
Exposes static methods to work with directories. It cannot be inherited. It performs some security checks on its methods, so, if we plan to use the same object several times, we'd probably want to use the `DirectoryInfo` class instead, as these checks will not be needed every time.

### Directory Class' Methods
Let's review some of the methods available in the `Directory` class. You can check the official documentation for [the full list](https://docs.microsoft.com/en-us/dotnet/api/system.io.directory?view=netframework-4.7.1#Methods).

- **CreateDirectory()**:  has overloads, but basically, creates a specified directory.
- **Delete()**: has overloads, but basically, deletes an empty directory or a directory and its content if specified.
- **Exists()**: returns whether a directory exists or not.
- **GetCurrentDirectory()**: gets the working directory of the application.
- **GetDirectories()**: has overloads, but basically returns the name of the subdirectories.
- **GetFiles()**: has overloads, but basically returns the name of the subfiles.
- **Move()**: moves a file or a directory with its content to a new location.
- **SetCurrentDirectory()**: sets the working directory for the application.

See below a quick sample on how to create a folder:

```csharp
Directory.CreateDirectory(@"c:\test\MyDir");
```

## File Class
Same as the Directory class, the `File` class provides static methods to work with files in this case. For one-time operations go ahead with the `File` class, but you may consider using the `FileInfo` instead if you do multiple operations with the same object to avoid the continuous checks that `File` class performs on every operation.

By default, full read / write access is granted to all users.

### File Class' Methods
Let's review some of the methods available in the `File` class. You can check the official documentation for [the full list](https://docs.microsoft.com/en-us/dotnet/api/system.io.file?view=netframework-4.7.1#Methods).

- **AppendAllLines()**: appends lines to a file.
- **AppendAllText()**: appends text to a file.
- **AppendText()**: creates a `StreamWriter` that appends UTF-8 encoded text.
- **Copy()**: copies an existing file.
- **Create()**: creates or overwrites a file.
- **CreateText()**: creates or opens a file for writing UTF-8 encoded text.
- **Delete()**: deletes a file.
- **Exists()**: returns whether the specified file exists or not.
- **Move()**: moves a file to a new location.
- **Open()**: opens a `FileStream` to a specified path.
 - **ReadAllLines()**: opens a file, reads all lines and closes the file.
- **ReadAllText()**: opens a text file, reads all lines and closes the text file.
- **ReadLines()**: reads the lines of a file.
- **Replace()**: replaces the contents of a file with the contents from another deleting the original file and creating a backup of the deleted one.
- **WriteAllLines()**: creates a new file, writes a collection of strings and closes the file.
- **WriteAllText()**: creates a new file, writes the specified string and closes the file.

Below a quick sample on how to create a file:

```csharp
string workingPath = @"C:\test";
string fileName = "MyFile.txt";
string filePath = Path.Combine(workingPath, fileName);
if (File.Exists(filePath) == false)
{
    // We first check if it Exists because the Create metehod
    // overwrites the existing one if any!
    File.Create(filePath);
    Console.WriteLine("{0} was created", fileName);
}
else
{
    Console.WriteLine("{0} already exists", fileName);
}
```

Here is how to add lines to the file above:

```csharp
string[] lines = new string[]
{
    "This is the first line",
    "This is the second line",
    "This is the third line",
};

// As we don't want to override the file in this sample
// we'll Append instead of Writing
File.AppendAllLines(filePath, lines);
Console.WriteLine("Contend added to {0}", filePath);
```

And this is how we would read the lines in the file above:

```csharp
var readLines = File.ReadAllLines(filePath);

Console.WriteLine("This is the content of the file:");
foreach(var line in readLines)
{
    Console.WriteLine(line);
}
```