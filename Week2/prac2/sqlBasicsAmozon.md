

help

```sql
CREATE TABLE Addresses (
    addressID INT IDENTITY PRIMARY KEY,
    addressLine VARCHAR(100) NOT NULL,
    suburb VARCHAR(50) NOT NULL,
    postcode VARCHAR(10) NOT NULL,
    region VARCHAR(50) NOT NULL,
    country VARCHAR(50) NOT NULL
);

CREATE TABLE Customers (
    customerID INT IDENTITY PRIMARY KEY,
    firstName VARCHAR(100) NOT NULL,
    lastName VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    mainPhoneNumber VARCHAR(15) NOT NULL,
    secondaryPhoneNumber VARCHAR(15),
    addressID INT,
    CONSTRAINT fk_address FOREIGN KEY (addressID) REFERENCES Addresses(addressID)
);

CREATE TABLE ItemCategories (
    categoryID INT IDENTITY PRIMARY KEY,
    parentCategoryID INT,
    categoryName VARCHAR(100) NOT NULL,
    CONSTRAINT fk_parentCategory FOREIGN KEY (parentCategoryID) REFERENCES ItemCategories(categoryID)
);

CREATE TABLE Items (
    itemID INT IDENTITY PRIMARY KEY,
    itemName VARCHAR(150) NOT NULL,
    itemDescription VARCHAR(MAX) NOT NULL,
    itemCost DECIMAL(10,2) NOT NULL,
    itemImage VARCHAR(MAX) NOT NULL,
    categoryID INT NOT NULL,
    CONSTRAINT fk_category FOREIGN KEY (categoryID) REFERENCES ItemCategories(categoryID)
);

CREATE TABLE CustomerOrders (
    orderNumber INT IDENTITY PRIMARY KEY,
    customerID INT NOT NULL,
    orderDate DATE NOT NULL DEFAULT GETDATE(),
    datePaid DATE,
    CONSTRAINT fk_customer FOREIGN KEY (customerID) REFERENCES Customers(customerID)
);

CREATE TABLE ItemsInOrder (
    orderNumber INT NOT NULL,
    itemID INT NOT NULL,
    numberOf INT NOT NULL DEFAULT 1,
    totalItemCost DECIMAL(10,2),
    CONSTRAINT pk_orderNumberNitemID PRIMARY KEY (orderNumber, itemID),
    CONSTRAINT fk_item FOREIGN KEY (itemID) REFERENCES Items(itemID),
    CONSTRAINT fk_orderNumber FOREIGN KEY (orderNumber) REFERENCES CustomerOrders(orderNumber)
);

CREATE TABLE Reviews (
    reviewID INT IDENTITY PRIMARY KEY,
    customerID INT NOT NULL,
    itemID INT NOT NULL,
    reviewDate DATE NOT NULL DEFAULT GETDATE(),
    rating INT NOT NULL CHECK (rating >= 1 AND rating <= 5),
    reviewDescription VARCHAR(MAX) NOT NULL,
    CONSTRAINT CK1 UNIQUE (customerID, itemID, reviewDate),
    CONSTRAINT fk_customer FOREIGN KEY (customerID) REFERENCES Customers(customerID),
    CONSTRAINT fk_items FOREIGN KEY (itemID) REFERENCES Items(itemID)
);

CREATE TABLE ItemMarkupHistory (
    itemID INT NOT NULL,
    startDate DATE NOT NULL,
    endDate DATE,
    markup DECIMAL(4,1) DEFAULT 1.3 NOT NULL,
    sale BIT DEFAULT 0,
    CONSTRAINT pk_itemNstartDate PRIMARY KEY (itemID, startDate),
    CONSTRAINT fk_item FOREIGN KEY (itemID) REFERENCES Items(itemID)
);

```

Question 1: Write a single INSERT statement for a review on an item of your choice.

```sql
INSERT INTO Reviews (customerID, itemID, reviewDate, rating, reviewDescription)
VALUES (1, 1, GETDATE(), 5, 'This item exceeded my expectations! High quality and fast shipping.');

```


Question 2: Write a DELETE statement to remove your review.

```sql
DELETE FROM Reviews
WHERE customerID = 1 AND itemID = 1 AND reviewDate = '2024-08-02';

```


Question 3: Find all the Items that cost 15 or less dollars.

```sql
SELECT * 
FROM Items 
WHERE itemCost <= 15;

```


Question 4: Find all the Customers whose first OR last name starts with 'And'.
```sql
SELECT * 
FROM Customers 
WHERE firstName LIKE 'And%' OR lastName LIKE 'And%';

```


Question 5: Find all the Customers who have a secondary phone number.

```sql
SELECT * 
FROM Customers 
WHERE secondaryPhoneNumber IS NOT NULL;

```




Question 6: Find all Addresses that are in South Australia.
```sql
SELECT * 
FROM Addresses 
WHERE region = 'South Australia';

```


Question 7: Find all the orders that still haven’t been paid (datePaid = NULL).

```sql
SELECT * 
FROM CustomerOrders 
WHERE datePaid IS NULL;

```



Question 8: Find all the orders that were paid on OR after the day they were ordered in 2023. Re-order the results from most recent orderDate to the oldest.
```sql
SELECT * 
FROM CustomerOrders 
WHERE datePaid >= orderDate 
  AND YEAR(orderDate) = 2023 
ORDER BY orderDate DESC;


```



Question 9: Find all unique regions, removing any duplications in alphabetical order.

```sql
SELECT DISTINCT region 
FROM Addresses 
ORDER BY region;

```




Question 10: Update the region/state information to the abbreviated version.

```sql
-- Update South Australia to SA
UPDATE Addresses 
SET region = 'SA' 
WHERE region = 'South Australia';

-- Update New South Wales to NSW
UPDATE Addresses 
SET region = 'NSW' 
WHERE region = 'New South Wales';

-- Update Victoria to VIC
UPDATE Addresses 
SET region = 'VIC' 
WHERE region = 'Victoria';

-- Update Queensland to QLD
UPDATE Addresses 
SET region = 'QLD' 
WHERE region = 'Queensland';

-- Update Western Australia to WA
UPDATE Addresses 
SET region = 'WA' 
WHERE region = 'Western Australia';

-- Update Northern Territory to NT
UPDATE Addresses 
SET region = 'NT' 
WHERE region = 'Northern Territory';

-- Update Australian Capital Territory to ACT
UPDATE Addresses 
SET region = 'ACT' 
WHERE region = 'Australian Capital Territory';

-- Update Tasmania to TAS
UPDATE Addresses 
SET region = 'TAS' 
WHERE region = 'Tasmania';

```



Question 11: Update all customer records so that if the phone number/email is blank, the value is replaced with a NULL value.

```sql
-- Update blank email values to NULL
UPDATE Customers 
SET email = NULL 
WHERE email = '';

-- Update blank main phone numbers to NULL
UPDATE Customers 
SET mainPhoneNumber = NULL 
WHERE mainPhoneNumber = '';

-- Update blank secondary phone numbers to NULL
UPDATE Customers 
SET secondaryPhoneNumber = NULL 
WHERE secondaryPhoneNumber = '';

```










