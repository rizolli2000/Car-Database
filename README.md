# Car-Database

-- Create the CarPurchase table
CREATE TABLE CarPurchase (
    PurchaseID INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    CarModel VARCHAR(100),
    PurchaseDate DATE,
    PurchasePrice DECIMAL(10, 2),
    Salesperson VARCHAR(100)
);

-- Insert sample data into the CarPurchase table
INSERT INTO CarPurchase (PurchaseID, CustomerName, CarModel, PurchaseDate, PurchasePrice, Salesperson)
VALUES
    (1, 'John Doe', 'Toyota Camry', '2024-02-01', 25000.00, 'Sarah Johnson'),
    (2, 'Alice Smith', 'Honda Civic', '2024-02-03', 22000.00, 'Mike Williams'),
    (3, 'Bob Brown', 'Ford Mustang', '2024-02-05', 35000.00, 'Emily Davis'),
    (4, 'Emma Johnson', 'BMW X5', '2024-02-07', 50000.00, 'John Smith'),
    (5, 'David Wilson', 'Tesla Model 3', '2024-02-08', 45000.00, 'Rachel Lee'),
    (6, 'Sophia Miller', 'Chevrolet Silverado', '2024-02-10', 40000.00, 'Chris Martinez'),
    (7, 'Olivia Taylor', 'Audi Q5', '2024-02-12', 48000.00, 'Daniel Jackson'),
    (8, 'James Anderson', 'Mercedes-Benz E-Class', '2024-02-14', 55000.00, 'Jessica Brown'),
    (9, 'Ethan White', 'Lexus RX', '2024-02-15', 42000.00, 'Amanda Wilson'),
    (10, 'Charlotte Garcia', 'Volkswagen Golf', '2024-02-18', 28000.00, 'Michael Johnson'),
    (11, 'Mia Martinez', 'Subaru Outback', '2024-02-20', 32000.00, 'Sarah Johnson'),
    (12, 'Noah Hernandez', 'Hyundai Sonata', '2024-02-22', 26000.00, 'Emily Davis'),
    (13, 'Isabella Lopez', 'Kia Sportage', '2024-02-24', 30000.00, 'Rachel Lee'),
    (14, 'William Perez', 'Jeep Wrangler', '2024-02-26', 38000.00, 'Chris Martinez'),
    (15, 'Amelia Gonzalez', 'Toyota Prius', '2024-02-28', 32000.00, 'Daniel Jackson'),
    (16, 'Evelyn Ramirez', 'Honda Accord', '2024-03-01', 29000.00, 'Jessica Brown'),
    (17, 'Michael Torres', 'Ford F-150', '2024-03-03', 45000.00, 'Amanda Wilson'),
    (18, 'Alexander Flores', 'Chevrolet Equinox', '2024-03-05', 33000.00, 'Michael Johnson'),
    (19, 'Ava Cruz', 'Tesla Model S', '2024-03-07', 75000.00, 'Sarah Johnson'),
    (20, 'Sophie Reed', 'Audi A4', '2024-03-10', 52000.00, 'Emily Davis');

-- Analytical Queries

-- 1. Total number of purchases
SELECT COUNT(*) AS TotalPurchases FROM CarPurchase;

-- 2. Total sales amount
SELECT SUM(PurchasePrice) AS TotalSalesAmount FROM CarPurchase;

-- 3. Average purchase price
SELECT AVG(PurchasePrice) AS AveragePurchasePrice FROM CarPurchase;

-- 4. Salesperson with the highest sales
SELECT Salesperson, SUM(PurchasePrice) AS TotalSalesAmount
FROM CarPurchase
GROUP BY Salesperson
ORDER BY TotalSalesAmount DESC
LIMIT 1;

-- 5. Most popular car model
SELECT CarModel, COUNT(*) AS TotalPurchases
FROM CarPurchase
GROUP BY CarModel
ORDER BY TotalPurchases DESC
LIMIT 1;


-- Analytical Queries with Window Functions

-- 1. Rank salespersons by total sales amount
SELECT 
    Salesperson,
    SUM(PurchasePrice) AS TotalSalesAmount,
    RANK() OVER (ORDER BY SUM(PurchasePrice) DESC) AS SalespersonRank
FROM CarPurchase
GROUP BY Salesperson;

-- 2. Calculate cumulative sales amount by date
SELECT 
    PurchaseDate,
    SUM(PurchasePrice) OVER (ORDER BY PurchaseDate) AS CumulativeSalesAmount
FROM CarPurchase
GROUP BY PurchaseDate
ORDER BY PurchaseDate;

-- 3. Calculate average purchase price by car model
SELECT 
    CarModel,
    AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS AveragePurchasePrice
FROM CarPurchase;

-- 4. Determine the difference between each purchase price and the average purchase price of the respective car model
SELECT 
    PurchaseID,
    CarModel,
    PurchasePrice,
    AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS AveragePurchasePrice,
    PurchasePrice - AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS PriceDifference
FROM CarPurchase;

-- 5. Rank customers by the number of purchases made
SELECT 
    CustomerName,
    COUNT(*) AS TotalPurchases,
    RANK() OVER (ORDER BY COUNT(*) DESC) AS CustomerRank
FROM CarPurchase
GROUP BY CustomerName;

-- Analytical Queries with Window Functions

-- 1. Rank salespersons by total sales amount
SELECT 
    Salesperson,
    SUM(PurchasePrice) AS TotalSalesAmount,
    RANK() OVER (ORDER BY SUM(PurchasePrice) DESC) AS SalespersonRank
FROM CarPurchase
GROUP BY Salesperson;

-- 2. Calculate cumulative sales amount by date
SELECT 
    PurchaseDate,
    SUM(PurchasePrice) OVER (ORDER BY PurchaseDate) AS CumulativeSalesAmount
FROM CarPurchase
GROUP BY PurchaseDate
ORDER BY PurchaseDate;

-- 3. Calculate average purchase price by car model
SELECT 
    CarModel,
    AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS AveragePurchasePrice
FROM CarPurchase;

-- 4. Determine the difference between each purchase price and the average purchase price of the respective car model
SELECT 
    PurchaseID,
    CarModel,
    PurchasePrice,
    AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS AveragePurchasePrice,
    PurchasePrice - AVG(PurchasePrice) OVER (PARTITION BY CarModel) AS PriceDifference
FROM CarPurchase;

-- 5. Rank customers by the number of purchases made
SELECT 
    CustomerName,
    COUNT(*) AS TotalPurchases,
    RANK() OVER (ORDER BY COUNT(*) DESC) AS CustomerRank
FROM CarPurchase
GROUP BY CustomerName;
