# Database
1.	Consider an online Car Rental model.

Requirements:

a.	Operations: 
Customer registration: Storing customer information such as name, email, address, and phone number.
Vehicle inventory management: Managing information about vehicles, such as make, model, year, license plate, category, and status.
Rental transactions: Recording transactions related to vehicle rentals, including pickup and return dates, and total amount charged.
Reservation management: Keeping track of reservation details, such as customer, vehicle, start and end dates, and total amount.
Vehicle maintenance tracking: Logging vehicle maintenance history, including maintenance date, description, and cost.

b.	Customer information: Name, email, address, and phone number.
Vehicle details: Make, model, year, license plate, category (e.g., sedan, SUV, convertible), and status (e.g., available, rented, maintenance).
Rental transactions: Rental ID, reservation ID, pickup date, return date, and total amount.
Reservation details: Reservation ID, customer ID, vehicle ID, start date, end date, and total amount.
Vehicle maintenance history: Maintenance ID, vehicle ID, maintenance date, description (e.g., oil change, tire replacement), and cost.

c.	Relationships and constraints:
One-to-many relationship between customers and reservations: A customer can have multiple reservations, but a reservation must be associated with only one customer.
One-to-many relationship between vehicles and reservations: A vehicle can have multiple reservations, but a reservation must be associated with only one vehicle.
One-to-one relationship between reservations and rental transactions: Each reservation corresponds to a single rental transaction.
One-to-many relationship between vehicles and maintenance records: A vehicle can have multiple maintenance records, but a maintenance record must be associated with only one vehicle.

Constraints:
A reservation must have a valid customer and vehicle.
A rental transaction must be associated with a valid reservation.
A maintenance record must be associated with a valid vehicle.
A vehicle's status must be one of the following: available, rented, or maintenance.

2.	ER Diagram

ER diagrams involve defining the entities and their attributes, it also includes relationships and cardinalities between entities. This provides a visual representation of the database schema, making it easier to understand the structure and design of the system.

 
Attached is the .erdplus file.
List of entities, attributes, and relationships is as follows:
a. Customer (CustomerID [PK], FirstName, LastName, Email, Address, Phone)
b. Vehicle (VehicleID [PK], Make, Model, Year, LicensePlate, Category, Status)
c. Reservation (ReservationID [PK], CustomerID [FK], VehicleID [FK], StartDate, EndDate, TotalAmount)
d. RentalTransaction (RentalID [PK], ReservationID [FK], PickupDate, ReturnDate, TotalAmount)
e. Maintenance (MaintenanceID [PK], VehicleID [FK], MaintenanceDate, Description, Cost)



3.	Relational Schema:

This involves defining tables (relations) and columns (attributes) based on the entities and attributes from the ER diagram
 
Attached is the .erdplus file.


4.	Referential Integrity Constraints:

These constraints ensure that relationships between tables remain consistent. 

Cascade delete for reservations when a customer is deleted.
Cascade delete for rental transactions when a reservation is deleted.
Restrict delete for vehicles with associated reservations or maintenance records.

5.	SQL Implementation:

CREATE TABLE Customer (
  CustomerID INT PRIMARY KEY AUTO_INCREMENT,
  FirstName VARCHAR(100),
  LastName VARCHAR(100),
  Email VARCHAR(100) ,
  Address VARCHAR(100),
  Phone VARCHAR(100)
);

CREATE TABLE Vehicle (
  VehicleID INT PRIMARY KEY AUTO_INCREMENT,
  Make VARCHAR(100),
  Model VARCHAR(100),
  Year INT,
  LicensePlate VARCHAR(15) UNIQUE,
  Category VARCHAR(100),
  Status VARCHAR(100)
);

CREATE TABLE Reservation (
  ReservationID INT PRIMARY KEY AUTO_INCREMENT,
  CustomerID INT,
  VehicleID INT,
  StartDate DATE,
  EndDate DATE,
  TotalAmount DECIMAL(10, 2),
  FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
  FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID)
);

CREATE TABLE RentalTransaction (
  RentalID INT PRIMARY KEY AUTO_INCREMENT,
  ReservationID INT,
  PickupDate DATE,
  ReturnDate DATE,
  TotalAmount DECIMAL(10, 2),
  FOREIGN KEY (ReservationID) REFERENCES Reservation(ReservationID)
);

CREATE TABLE Maintenance (
  MaintenanceID INT PRIMARY KEY AUTO_INCREMENT,
  VehicleID INT,
  MaintenanceDate DATE,
  Description VARCHAR(255),
  Cost DECIMAL(10, 2),
  FOREIGN KEY (VehicleID) REFERENCES Vehicle(VehicleID)
);

-- Insert 10 records into Customer table
INSERT INTO Customer (FirstName, LastName, Email, Address, Phone)
VALUES
('John', 'Doe', 'johndoe@email.com', '123 Main St, Cityville', '555-123-4567'),
('Jane', 'Smith', 'janesmith@email.com', '456 Elm St, Townburg', '555-987-6543'),
('Michael', 'Johnson', 'michaelj@email.com', '789 Oak St, Villageplace', '555-321-0987'),
('Sara', 'Lee', 'saralee@email.com', '1011 Pine St, Metropolis', '555-555-1212'),
('David', 'Brown', 'davidb@email.com', '1213 Willow St, Hamlet', '555-888-7777'),
('Laura', 'Martinez', 'lauram@email.com', '1415 Maple St, Springfield', '555-909-8080'),
('Mark', 'Anderson', 'marka@email.com', '1617 Cypress St, Gotham', '555-333-4444'),
('Stephanie', 'Adams', 'stephaniea@email.com', '1819 Birch St, Utopia', '555-222-1111'),
('Paul', 'Roberts', 'paulr@email.com', '2021 Cedar St, Westeros', '555-444-3333'),
('Lisa', 'Taylor', 'lisat@email.com', '2223 Cherry St, Smalltown', '555-666-5555');

-- Insert 10 records into Vehicle
INSERT INTO Vehicle (Make, Model, Year, LicensePlate, Category, Status)
VALUES
('Toyota', 'Camry', 2020, 'ABC123', 'sedan', 'available'),
('Honda', 'Accord', 2019, 'DEF456', 'sedan', 'rented'),
('Ford', 'Mustang', 2018, 'GHI789', 'convertible', 'maintenance'),
('Chevrolet', 'Tahoe', 2021, 'JKL012', 'SUV', 'available'),
('Nissan', 'Altima', 2020, 'MNO345', 'sedan', 'rented'),
('Jeep', 'Wrangler', 2019, 'PQR678', 'SUV', 'available'),
('BMW', '3 Series', 2021, 'STU901', 'convertible', 'available'),
('Audi', 'A4', 2020, 'VWX234', 'sedan', 'rented'),
('Mercedes-Benz', 'C-Class', 2018, 'YZA567', 'convertible', 'available'),
('Lexus', 'RX', 2021, 'BCD890', 'SUV', 'maintenance');

-- Insert 10 records into Reservation
INSERT INTO Reservation (CustomerID, VehicleID, StartDate, EndDate, TotalAmount)
VALUES
(1, 1, '2023-05-10', '2023-05-15', 300),
(2, 2, '2023-05-12', '2023-05-14', 200),
(3, 3, '2023-05-20', '2023-05-25', 400),
(4, 4, '2023-05-08', '2023-05-11', 450),
(5, 5, '2023-05-15', '2023-05-18', 250),
(6, 6, '2023-05-18', '2023-05-23', 600),
(7, 7, '2023-05-22', '2023-05-24', 350),
(8, 8, '2023-05-06', '2023-05-08', 275),
(9, 9, '2023-05-30', '2023-06-04', 475),
(10, 10, '2023-06-10', '2023-06-15', 550);

-- Insert 10 records into RentalTransaction
INSERT INTO RentalTransaction (ReservationID, PickupDate, ReturnDate, TotalAmount)
VALUES
(1, '2023-05-10', '2023-05-15', 300),
(2, '2023-05-12', '2023-05-14', 200),
(3, '2023-05-20', '2023-05-25', 400),
(4, '2023-05-08', '2023-05-11', 450),
(5, '2023-05-15', '2023-05-18', 250),
(6, '2023-05-18', '2023-05-23', 600),
(7, '2023-05-22', '2023-05-24', 350),
(8, '2023-05-06', '2023-05-08', 275),
(9, '2023-05-30', '2023-06-04', 475),
(10, '2023-06-10', '2023-06-15', 550);


-- Insert 10 records into Maintenance
INSERT INTO Maintenance (VehicleID, MaintenanceDate, Description, Cost)
VALUES
(3, '2023-04-15', 'Oil change', 50),
(3, '2023-02-10', 'Tire rotation', 60),
(10, '2023-05-01', 'Brake pad replacement', 200),
(4, '2023-03-20', 'Battery replacement', 150),
(7, '2023-04-05', 'Transmission fluid change', 100),
(5, '2023-01-15', 'Coolant flush', 80),
(9, '2023-02-28', 'Spark plug replacement', 110),
(8, '2023-04-30', 'Wheel alignment', 90),
(2, '2023-03-15', 'Air filter replacement', 30),
(6, '2023-05-01', 'Tire replacement', 400);

6.	Data Warehouse Requirements:
	![image](https://github.com/sravanit9559/Database/assets/130037004/f804e3da-e45f-48c2-aa05-239a6bea9445)
![image](https://github.com/sravanit9559/Database/assets/130037004/4087bb72-3d42-4d6c-bad3-f01ad81326df)


A data warehouse is a large, centralized repository of data used for data reporting and analysis. In This case we identify the subject of analysis, the attributes that will be useful for analysis, and the granularity of the facts.

a.	Subject of analysis: 
The subject of analysis is the performance and revenue generation of the car rental service, focusing on factors such as customer preferences, vehicle utilization, and rental transaction revenue.

b.	Fields/attributes: 
The useful fields or attributes in the analysis includes customer demographics (age, location, etc.), vehicle details (make, model, year, category, status), rental transaction details (reservation dates, rental duration, total amount), and date/time information (day, month, quarter, year).

c.	Granularity: 
The granularity of the facts/measures in this analysis is at the individual rental transaction level. This means that each record in the fact table represents a single rental transaction, capturing details about the customer, the vehicle, the reservation, and the revenue generated from that transaction.

7.	Star Schema of Data Warehouse:
	![image](https://github.com/sravanit9559/Database/assets/130037004/334022d7-554a-4e9a-a82a-e3a993eae84d)


A star schema consists of a central fact table surrounded by dimension tables. Each dimension table is connected to the fact table via a primary key-foreign key relationship. This schema simplifies queries and improves performance by reducing the number of joins required for analysis.

  
Attached is the .erdplus file.
