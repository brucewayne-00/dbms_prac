-- STEP 1: Drop existing objects
DROP VIEW IF EXISTS CustomerAccountView;
DROP VIEW IF EXISTS LoanCustomerView;
DROP TABLE IF EXISTS `Transaction`, Loan, Account, Branch, Customer;
DROP INDEX CustomerId ON Customer;

-- STEP 2: Create tables
CREATE TABLE Customer (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Address VARCHAR(255),
    Phone VARCHAR(15)
);

CREATE TABLE Branch (
    BranchID INT PRIMARY KEY,
    BranchName VARCHAR(100) NOT NULL,
    Location VARCHAR(100)
);

CREATE TABLE Account (
    AcctNo INT PRIMARY KEY AUTO_INCREMENT,
    Balance DECIMAL(12,2) NOT NULL,
    Type ENUM('Saving', 'Current', 'Fixed') NOT NULL,
    BranchID INT,
    CustomerID INT,
    FOREIGN KEY (BranchID) REFERENCES Branch(BranchID),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

CREATE TABLE Loan (
    LoanID INT PRIMARY KEY AUTO_INCREMENT,
    Amount DECIMAL(12,2) NOT NULL,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

CREATE TABLE `Transaction` (
    TxnID INT PRIMARY KEY AUTO_INCREMENT,
    Date DATE NOT NULL,
    Amount DECIMAL(12,2) NOT NULL,
    AcctNo INT,
    FOREIGN KEY (AcctNo) REFERENCES Account(AcctNo)
);

-- STEP 3: Create views
CREATE VIEW CustomerAccountView AS
SELECT c.CustomerID, c.Name, c.Phone, a.AcctNo, a.Type, a.Balance
FROM Customer c
JOIN Account a ON c.CustomerID = a.CustomerID;

CREATE VIEW LoanCustomerView AS
SELECT l.LoanID, l.Amount, c.CustomerID, c.Name, c.Phone
FROM Loan l
JOIN Customer c ON l.CustomerID = c.CustomerID;

-- STEP 4: Create index
CREATE INDEX CustomerId ON Customer(Name ASC);
