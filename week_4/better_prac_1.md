

uestion 1: Using an aggregate function, find the date of the earliest customer order.
```sql

SELECT MIN(orderDate) AS earliestOrderDate

FROM CustomerOrders;
```
Question 2: Find the names of all customers that placed an order on the same day as the earliest customer order.
```sql

SELECT C.firstName, C.lastName

FROM Customers AS C

JOIN CustomerOrders AS O ON C.customerID = O.customerID

WHERE O.orderDate = (

    SELECT MIN(orderDate)
    
    FROM CustomerOrders

);
```
Question 3: Find the total cost of each customer order showing the order number and total value for the above earliest date. Sort the values in order of increasing total value.
```sql
SELECT IO.orderNumber, SUM(IO.totalItemCost) AS totalValue

FROM ItemsInOrder AS IO

JOIN CustomerOrders AS O ON IO.orderNumber = O.orderNumber

WHERE O.orderDate = (

    SELECT MIN(orderDate)
    
    FROM CustomerOrders

)

GROUP BY IO.orderNumber

ORDER BY totalValue ASC;
```
Question 4: Find the average cost of all orders made on the earliest order date.
```sql
SELECT AVG(totalValue) AS averageOrderCost

FROM (

    SELECT SUM(IO.totalItemCost) AS totalValue
    
    FROM ItemsInOrder AS IO
    
    JOIN CustomerOrders AS O ON IO.orderNumber = O.orderNumber
    
    WHERE O.orderDate = (
    
        SELECT MIN(orderDate)
        
        FROM CustomerOrders
    
    )
    
    GROUP BY IO.orderNumber

) AS orderTotals;
```
Question 5: Show the customer name and number of orders each customer has made grouping only by customerID. Don’t forget to convert NULL to “0” and order the results by the number of orders increasing.
```sql
SELECT C.firstName, C.lastName, ISNULL(COUNT(O.orderNumber), 0) AS numberOfOrders

FROM Customers AS C

LEFT JOIN CustomerOrders AS O ON C.customerID = O.customerID

GROUP BY C.customerID, C.firstName, C.lastName

ORDER BY numberOfOrders ASC;
```
Question 6: Show the names of people who have made the most orders (i.e., different order numbers for a given customer).
```sql
SELECT C.firstName, C.lastName

FROM Customers AS C

JOIN CustomerOrders AS O ON C.customerID = O.customerID

GROUP BY C.customerID, C.firstName, C.lastName

HAVING COUNT(O.orderNumber) = (

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
SELECT I.itemName, SUM(IO.numberOf) AS totalPurchased

FROM Items AS I

JOIN ItemsInOrder AS IO ON I.itemID = IO.itemID

GROUP BY I.itemID, I.itemName

ORDER BY totalPurchased DESC

LIMIT 1;
These queries are formatted to match your preferred programming style.
```
