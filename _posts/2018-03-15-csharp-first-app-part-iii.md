---
layout: post
title:  C# First App (Part III)
date:   2018-03-15 07:00:00 +0100
categories: software development
---
In the previous iteration of our Car Rental Agency software management we added new services that allowed us to store and get data using the File System. Now, we will work on implementing Interfaces and adding Dependency Injection (the concept itself, but we will not set up and use any Dependency Injection Container).
<!--more-->

## Interfaces

### Service Layer
Create the following Interfaces within the Service folder we have in our project:
- ICarService
- ICustomerService
- IRentalService

Take into account that both, Cars and Customers have 2 services in the application, therefore, the two services for Cars (`CarService` and `CarFileService` ) shall implement the `ICarService` interface and be compliant with it; in the same way both services for Customers have to be compliant with the `ICustomerService` intercace. In the case of Rentals, there is only one service (because in the first iteration we skipped the in-memory service for Rentals), which shall implement the `IRentalService`.

### Logic Layer
In the Logic layer we will create an interface per current Manager (Cars, Customers and Rentals, which will have to implement their corresponding Interface). Therefore, we will create the following Interfaces:
- ICarsManager
- ICustomersManager
- IRentalsManager

## Dependency Injection
Manager classes have to be updated in the following 2 ways:
1. Declare fields referring to other Managers or Services using the appropiate Interface instead of the Class as it is now
For Instance, instead of declaring a reference to the CarsManager inside our RentalsManager as follows
```csharp
CarsManager carsManager;
```
we would do it with the Interface instead:
```csharp
ICarsManager carsManager;
```
This has to be done anywhere were we are declaring any of the classes that now implement an interface (like in Program!)

2. Pass dependencies by Constructor and in the form of Interfaces. We have two cases now, one where dependencies are being injected as classes, and another scenario where dependencies are directly instantiated in the Constructor. Instead of any of these two scenarios, we have to inject them as Interfaces.

*Note that this post belongs to the [C# Introduction Series]({{ site.baseurl }}{% post_url 2018-01-30-csharp-series-introduction %}) and that the code is hosted in this [Github repo](https://github.com/nereolopez/csharp-intro).
The code relevant for this post is under the [Car Rental Agency folder](https://github.com/nereolopez/csharp-intro/tree/master/CarRentalAgency).*