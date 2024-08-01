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










