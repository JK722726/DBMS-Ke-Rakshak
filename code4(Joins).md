5. Program / SQL
5.1 Creating Base Tables
CREATE TABLE Customer ( 
CustomerID INT PRIMARY KEY,
CustomerName VARCHAR(100), 
Email VARCHAR(100)
);

CREATE TABLE Orders ( 
OrderID INT PRIMARY KEY,
CustomerID INT, 
OrderDate DATE, 
Amount DECIMAL(10,2),
FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

5.2 INNER JOIN
Retrieve customers who have placed orders.
SELECT
C.CustomerID, C.CustomerName,
O.OrderID, O.OrderDate, O.Amount
FROM Customer C 
INNER JOIN Orders O
ON C.CustomerID = O.CustomerID;

5.3 LEFT JOIN
Retrieve all customers including those with no orders.
SELECT
C.CustomerID, C.CustomerName, 
O.OrderID, O.OrderDate, O.Amount
FROM Customer C 
LEFT JOIN Orders O
ON C.CustomerID = O.CustomerID;

5.4 RIGHT JOIN
Retrieve all orders, including those that have no matching customer.
SELECT
C.CustomerID, C.CustomerName, 
O.OrderID, O.OrderDate, O.Amount
FROM Customer C 
RIGHT JOIN Orders O
ON C.CustomerID = O.CustomerID;

5.5 FULL OUTER JOIN
Retrieve all data, whether matched or unmatched.
SELECT
C.CustomerID, C.CustomerName, 
O.OrderID, O.OrderDate, O.Amount
FROM Customer C
LEFT JOIN Orders O
ON C.CustomerID = O.CustomerID UNION
SELECT
C.CustomerID, C.CustomerName, 
O.OrderID, O.OrderDate, O.Amount
FROM Customer C 
RIGHT JOIN Orders O
ON C.CustomerID = O.CustomerID;