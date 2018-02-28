---
layout: post
title:  "C# First App (Part I)"
date:   2018-02-22 09:00:00 +0100
categories: software development
---
# Our First App

We will add a new project to our solution where we will create an app to put into practice all that we've learnt so far. Our application will be a Car Rental Agency Software Management.

*Note: keep in mind that we will iterate over this application to add more things as we discuss more topics on C#, which means that some requirements might not be fulfilled right now.*

## Requirements
- No need for UI. A console app is enough
- Our app will manage three things: 
    - The cars we have (add new ones, list existing ones and remove existing one)
    - Customers (add new ones, list existing ones and remove existing one)
    - Rentals (both past and ongoing ones)
- Regarding Rentals, we need to care about the following :
    - Have a status (ongoing or done. **We will not consider reservations for the future or if a car ir returned late**).
    - Start a Rental
        - Taken cars cannot be selected
        - Customer and Car data are taken 
        - Register Start and End Date **(don't focus too much on validations yet, like making sure the end date if after the start date)**
        - Customer's Credit Card Taken and Deposit blocked from it.
        - Block Car to prevent it is offered to a simultaneous rental.
    - End a Rental **(so far create not implemented methods you think you'll need. We will do it later)**
        - Check Car status (if it had any damage) and unblock deposit if car is ok
        - Charge the rental fee (days rented  * car's price per day)
        - Charge Extra Kms / Miles if needed
        - Make Car available for another rental.

## Tech Specs
The Program class when creating the new project will serve to guide the user back and forth to do the actions. For each of the other things, so far, let's create a class for each of them with the needed members to keep their state and behavior.

We will split the code in three folders inside our project:
- **Logic**: where the objects for the Business Logic will be placed.
- **Services**: where the classes that deal with getting and sending data to the persistance will be placed.
- **Model**: where the classes that represent our business objects, their data and behavior will be placed.

### The Model

#### BaseModel Class
- Field-backed Property: Id of type string
- Constructor with optional parameter Id (if not provided, Id will be set to `Guid.NewGuid().ToString()`).

#### Energy Type Enum
It contains Gas, Dieasel, Hybrid and Electric as options.

#### Rental Status Enum
It contains Ongoing and Done as options.

#### Car class
Inherits from BaseModel.

##### Car Properties
Below you will see the needed properties and what type of data they will hold. Choose the right type for each.
- Maker (text)
- Model (text)
- EnergyType
- Kms (number)
- PricePerDay (money)
- MaxKmsPerDay (number)
- PricePerExtraKm (money)
- DepositFee (money)
- IsAvailable (true or false)

##### Constructors
- Constructor with optional parameter for the Id. It shall call the Base Class' constructor and also initialize the needed fields when creating a new car.
- Constructor receiving all the Car needed fields as parameters being the Id optional. It calls the previous constructor and sets all the Car's fields.

##### Car Methods
- Block. Does not return anything, just sets the Car as blocked to make sure nobady can rent it now.
- override the ToString() method all objects have to create and return a line of text with all the Car's fields separated by semicolon ' ;`. This will be used later when we add File Storage.

#### Customer Class
Inherits from BaseModel.

##### Customer Properties
Below you will see the needed properties and what type of data they will hold. Choose the right type for each.
- Name (text)
- LastName (text)
- Birthdate (date)
- DrivingLicense (text as driving license could have numbers and letters too)

##### Customer Constructor
- Constructor with optional parameter fo rthe Id that only calls the Base class' constructor.
- Constructor with all the fields of the Customer as parameters being the Id optional. It calls the previous constructor and sets all the Customer's fields.

#### Rental Class
Inherits from BaseModel.

##### Rental Properties
- CarId (text)
- CustomerId (text)
- StartDate (date)
- End Date (date)
- NumberOfDays (number). **This is an autocalculated property based on the StardDate and EndDate. This property is not backed by any field.**
- CarInitialKms (number)
- Fee (money)
- DepositFee (money)
- There will not be a property for the credit card number used to block the deposit as we don't want to expose it.
- Status
- CarDamaged (true or false)
- CarFinalKms (number)
- ExtraKmsFee (money)
- FinalTotal (money)

##### Rental Constructors
There are three constructors in this case, each of them for a different scenario. I place their signature here to ease it a bit:

First Constructor
```csharp
private Rental(string id = null, RentalStatus status = RentalStatus.Ongoing) :base(id)
```

Second Constructor (used when creating a new Rental)
```csharp
 public Rental(Car car, string customerId, DateTime startDate, DateTime endDate, int creditCardNumber, string id = null) :this(id)
```
Third Constructor (used when loading data from the Data Source. This won't  be used until the next iteration on this app)
```csharp
public Rental(string id, string carId, string customerId, DateTime startDate, DateTime endDate, int carInitialKms, decimal fee, decimal depositFee,
            int creditCardNumber, RentalStatus status, bool carDamaged, int carFinalKms, decimal extraKmsFee, decimal finalTotal):
            this(id, status)
```

##### Rental Methods
- override the ToString() method all objects have to create and return a line of text with all the Rental's fields (fields! not properties) separated by semicolon ' ;`. This will be used later when we add File Storage.

### The Business Logic
These are the classes that will be inside the Logic folder we mentioned before.

#### CarsManager Class
This class contains all the logic related to Cars management.

##### CarsManager Fields and Properties
- It will have a field of type `CarService`
- List of Cars (all the cars). This list will be gotten from the `CarService`
- List of Cars (only the available ones). This will be gotten from the `CarService`.

##### CarsManager Constructor
It will instantiate a `CarService` and store it on its declared field.

##### CarsManager Methods
- Show Cars will display on screen all the cars the company has
- Show Available Cars will display on screen all the cars the company has that are available (not rented)
- Add Car (no need to implement it yet)
- Block Car asks the `CarService` instance to block a given car
- Remove Car (no need to implement it yet)

Feel free to create any additional method (private in this case) that could help you with the public ones defined above.

#### CustomersManager Class
This class contains all the logic related to Cars management.

##### CustomersManager Fields and Properties
- It will have a field of type `CustomerService`
- List of Customers (all the customers). This list will be gotten from the `CustomerService`

##### Customers Constructor
It will instantiate a `CustomerService` and store it on its declared field.

##### Customers Methods
- Show Customers will display on screen all the customers the company has
- Add Customer (no need to implement it yet)

Feel free to create any additional method (private in this case) that could help you with the public ones defined above.

#### RentalsManager Class
This class contains all the logic related to Cars management.

##### RentalsManager Fields and Properties
- It will have a field of type `CarsManager`
- It will have a field of type `CustomersManager`
- List of Rentals

##### RentalsManager Constructor
It will instantiate a `CarsManager`, `CustomersManager` and store it on its declared field. It will also instantiate the Rentals List.
**Remember that we don't have Storage yet, but, in the case of the Rentals, we won't even have in this first version initial fake data.**

##### RentalsManager Methods
- Add Rental, which will gather all the information needed to create a Rental, add the created Rental to the Rental List and call the `ShowAddedRentalInformation()`.
- Show Added Rental Information will display on screen the information of a given Rental.

You will probably want the following private methods:
- Select Customer
- Select Car
- Select Date
- Ask for Credit Card Number

### The Services

#### CarService Class

##### CarService Fields
- List of Cars

##### CarService Constructor
Initializes the List of Cars and calls the `CreateHardcodedCars()` method which will add 5 cars to the List of Cars.

##### CarService Methods
- Get Cars returns the List of Cars
- Block Car sets the given Car as blocked (the update takes effect on the Car within the List of Cars!)
- Create Hardcoded Cars  (private) adds 5 hardcoded cars the List of Cars.

#### CustomerService Class

##### CustomerService Fields
- List of Customers

##### CustomerService Constructor
Initializes the List of Customers and calls the `CreateHardcodedCustomers()` method which will add 4 customers to the List of Customers.

##### CustomerService Methods
- Get Customers returns the List of Cutsomers
- Create Hardcoded Customers  (private) adds 4 hardcoded customers the List of Customers