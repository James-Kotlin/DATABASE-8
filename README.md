# DATABASE-8
Database for a shop

-- Create Customers Table
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    Email VARCHAR(255) UNIQUE NOT NULL,
    Phone VARCHAR(15),
    Address TEXT
);

-- Create Products Table
CREATE TABLE Products (
    ProductID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    Description TEXT,
    Price DECIMAL(10,2) NOT NULL,
    StockQuantity INT NOT NULL
);

-- Create Orders Table
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT,
    CustomerID INT NOT NULL,
    OrderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    TotalAmount DECIMAL(10,2),
    Status ENUM('Pending', 'Shipped', 'Delivered', 'Cancelled') DEFAULT 'Pending',
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Create OrderDetails Table (Many-to-Many Relationship Between Orders and Products)
CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY AUTO_INCREMENT,
    OrderID INT NOT NULL,
    ProductID INT NOT NULL,
    Quantity INT NOT NULL CHECK (Quantity > 0),
    Subtotal DECIMAL(10,2),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

-- Insert Sample Data
INSERT INTO Customers (Name, Email, Phone, Address)
VALUES 
    ('John Doe', 'john@example.com', '1234567890', '123 Main Street'),
    ('Jane Smith', 'jane@example.com', '0987654321', '456 Elm Street');

INSERT INTO Products (Name, Description, Price, StockQuantity)
VALUES
    ('Laptop', 'High-performance laptop', 1200.00, 10),
    ('Mouse', 'Wireless optical mouse', 20.00, 50),
    ('Keyboard', 'Mechanical gaming keyboard', 75.00, 30);

INSERT INTO Orders (CustomerID, TotalAmount, Status)
VALUES
    (1, 1220.00, 'Pending'),
    (2, 95.00, 'Shipped');

INSERT INTO OrderDetails (OrderID, ProductID, Quantity, Subtotal)
VALUES
    (1, 1, 1, 1200.00),
    (1, 2, 1, 20.00),
    (2, 3, 1, 75.00);

-- Query to Retrieve Order Details with Customer Information
SELECT o.OrderID, c.Name AS CustomerName, p.Name AS ProductName, od.Quantity, od.Subtotal
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
JOIN OrderDetails od ON o.OrderID = od.OrderID
JOIN Products p ON od.ProductID = p.ProductID;
