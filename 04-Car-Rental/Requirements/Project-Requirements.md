# Project 4: Car Rental Management System

## Project Requirements

### 1. Customer Management:
* The system should save customers' personal information: Name, Contact Information, and Driver's License Number.

### 2. Vehicles Information:
* The system should maintain up-to-date information on available vehicles, including: make, model, year, mileage, rental rates, fuel type (Gasoline, Diesel, Electric, Hybrid), plate number, and Vehicle Category (Sedan, 4x4, SUV, etc.).
* Vehicle Fuel Types:
  * Gasoline (Petrol)
  * Diesel
  * Electric
  * Hybrid

### 3. Vehicle Booking:
* When a customer rents a vehicle, the system should track booking details:
  * Customer renting the vehicle
  * Vehicle details
  * Rental start date and end date
  * Pickup location and drop-off location
  * Initial rental days
  * Initial total due amount
  * Initial vehicle check notes

### 4. Rental Transaction:
* The customer should pay for the rent, and a transaction should be logged in the system tracking:
  * Payment Details
  * Initial paid amount

### 5. Vehicle Return:
* When a customer returns a vehicle, the system should:
  * Record the actual return date
  * Calculate actual rental days
  * Record final vehicle check notes
  * Specify any additional charges
  * Record the current mileage and calculate the mileage consumed by the customer during the rental period
* The original transaction should be updated to:
  * Record differences from the initial reservation
  * Calculate the actual final amount due
  * Calculate the remaining amount due or refund amount if applicable
