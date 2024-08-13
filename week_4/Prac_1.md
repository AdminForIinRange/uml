Question 1: Using an aggregate function, find the date of the earliest customer order.
```sql

SELECT MIN(orderDate) AS earliestOrderDate
FROM CustomerOrders;
```
Question 2: Find the names of all customers that placed an order on the same day as the earliest customer order.
```sql

SELECT c.firstName, c.lastName
FROM Customers c
JOIN CustomerOrders o ON c.customerID = o.customerID
WHERE o.orderDate = (
    SELECT MIN(orderDate)
    FROM CustomerOrders
);
```
Question 3: Find the total cost of each customer order showing the order number and total value for the above earliest date. Sort the values in order of increasing total value.
```sql

SELECT iio.orderNumber, SUM(iio.totalItemCost) AS totalValue
FROM ItemsInOrder iio
JOIN CustomerOrders o ON iio.orderNumber = o.orderNumber
WHERE o.orderDate = (
    SELECT MIN(orderDate)
    FROM CustomerOrders
)
GROUP BY iio.orderNumber
ORDER BY totalValue ASC;
```
Question 4: Find the average cost of all orders made on the earliest order date.
```sql

SELECT AVG(totalValue) AS averageOrderCost
FROM (
    SELECT SUM(iio.totalItemCost) AS totalValue
    FROM ItemsInOrder iio
    JOIN CustomerOrders o ON iio.orderNumber = o.orderNumber
    WHERE o.orderDate = (
        SELECT MIN(orderDate)
        FROM CustomerOrders
    )
    GROUP BY iio.orderNumber
) AS orderTotals;
```
Question 5: Show the customer name and number of orders each customer has made grouping only by customerID. Don’t forget to convert NULL to “0” and order the results by the number of orders increasing.
```sql

SELECT c.firstName, c.lastName, ISNULL(COUNT(o.orderNumber), 0) AS numberOfOrders
FROM Customers c
LEFT JOIN CustomerOrders o ON c.customerID = o.customerID
GROUP BY c.customerID, c.firstName, c.lastName
ORDER BY numberOfOrders ASC;
```
Question 6: Show the names of people who have made the most orders (i.e., different order numbers for a given customer).
```sql

SELECT c.firstName, c.lastName
FROM Customers c
JOIN CustomerOrders o ON c.customerID = o.customerID
GROUP BY c.customerID, c.firstName, c.lastName
HAVING COUNT(o.orderNumber) = (
    SELECT MAX(orderCount)
    FROM (
        SELECT COUNT(orderNumber) AS orderCount
        FROM CustomerOrders
        GROUP BY customerID
    ) AS subquery
);
```
Question 7: Find the item(s) that has been purchased the most.
```sql

SELECT i.itemName, SUM(iio.numberOf) AS totalPurchased
FROM Items i
JOIN ItemsInOrder iio ON i.itemID = iio.itemID
GROUP BY i.itemID, i.itemName
ORDER BY totalPurchased DESC
LIMIT 1;
