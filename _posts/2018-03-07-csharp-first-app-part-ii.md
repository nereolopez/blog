---
layout: post
title:  C# First App (Part II)"
date:   2018-03-07 08:00:00 +0100
categories: software development
---
*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is under the [Car Rental Agency folder](https://github.com/nereolopez/csharp-intro/tree/master/CarRentalAgency).*

We will update the Car Rental Agency software management we started few sessions by making the services work with data coming from File Storage. As this time we will be using File Storage, the following classes within the Services folder will be created:
- CarFileService
- CustomerFileService
- RentalFileService

We will do the following in each of them:

1. Create a function that everytime the service is instantiated, if the file the service manages doesn't exist, it creates it and adds sample hardcoded data already to it (except in the RentalsFileService that will not add initial data).
2. The methods the previous services have are to be created in the new ones.
3. We will create the basic operations for each of them
    1. Write: Write each entry as a line and separate fields by semicolon `;`.
    
    ```csharp
    // This is a Customer object in the application
    var customer = new Customer();
    customer.Name = "Bob";
    customer.LastName = "Robson";
    customer.Birthdate = new DateTime(1980, 5, 28);
    customer.DrivingLicense = "436475J";

    // This is how its line should be written in Customers.txt
    Bob;Robson;28/05/1980;436475J
    ```
    
    2. Read: Each line represents an entry. Parse the line to the corresponding object and fill its properties while reading.
4. Change the application in a way that it updates the files instead of the in-memory collections we were using so far(in other words, make sure the old services are not used any longer).