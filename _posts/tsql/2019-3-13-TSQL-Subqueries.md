---
layout : post
title : T-SQL - Working with subqueries and APPLY
categories: [tsql]
tags: [tsql, sub-queries, nested sql, nested queries, apply, correlated queries, correlated, sql, data, sql server, database, AdventureWorks]
---
<hr />

``Note : The following scripts are the part of the course on edx titled as: Querying Data with Transact-SQL - ``  
[Querying Data with Transact-SQL](https://www.edx.org/course/querying-data-with-transact-sql-0)  
`` These queries works on AdventureWorks database and information regarding the same ``  
`` can be accessed by visiting following link - ``  
[AdventureWorks Installation and configuration](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017)
<hr />


```sql
/* Sub Queries - nested queries
Scalar subquery returns single values.
Multi values subquery return mutliple values as a single column which can be access using IN
*/

Select Max(UnitPrice) from SalesLT.SalesOrderDetail

Select * from SalesLT.Product where ListPrice > (Select Max(UnitPrice) From SalesLT.SalesOrderDetail)

/* Self Contained - Correlated
Most subqueries are self-contained and have no connection with outer query other than passing results
where as
correlated sub queries refers to elements of tables used in outer query, behaves as inner query executed
once per outer row */

Select CustomerID, SalesOrderID, OrderDate From SalesLT.SalesOrderHeader as S01

Select CustomerID, SalesOrderID, OrderDate From SalesLT.SalesOrderHeader as S01
where OrderDate = (Select Max(OrderDate) From SalesLT.SalesOrderHeader)


Select CustomerID, SalesOrderID, OrderDate From SalesLT.SalesOrderHeader as S01
Where orderdate = (Select Max(orderdate) from SalesLT.SalesOrderHeader As S02 where S02.CustomerID = S01.CustomerID)
Order By CustomerID


/* Apply operator
Cross Apply -  Apply right table expression to each row in left table - Conceptually lik Outer Join
Outer Apply -  add rows for thoses with null in columns for right table - conceptually like Left Outer Join


Select SOH.SalesOrderID, MUP.MaxUnitPrice From SalesLT.SalesOrderHeader as SOH Cross Apply SalesLT.udfMaxUnitPrice(SOH.SalesOrderID) as MUP Order BY SOH.SalesOrderID

*/

-- Setup
CREATE FUNCTION SalesLT.udfMaxUnitPrice (@SalesOrderID int)
RETURNS TABLE
AS
RETURN
SELECT SalesOrderID,Max(UnitPrice) as MaxUnitPrice FROM
SalesLT.SalesOrderDetail
WHERE SalesOrderID=@SalesOrderID
GROUP BY SalesOrderID;

--Display the sales order details for items that are equal to
-- the maximum unit price for that sales order
SELECT * FROM SalesLT.SalesOrderDetail AS SOH
CROSS APPLY SalesLT.udfMaxUnitPrice(SOH.SalesOrderID) AS MUP
WHERE SOH.UnitPrice=MUP.MaxUnitPrice
ORDER BY SOH.SalesOrderID;
```
<!--break-->

<hr>
