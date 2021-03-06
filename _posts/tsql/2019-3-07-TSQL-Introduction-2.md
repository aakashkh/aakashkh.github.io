---
layout : post
title : T-SQL - Introduction (Part 2)
categories: [tsql]
tags: [tsql, select, sql, data, sql server, database, AdventureWorks, duplicates, sorting, paging, filter, like]
---
<hr />

``Note : The following scripts are the part of the course on edx titled as: Querying Data with Transact-SQL - ``  
[Querying Data with Transact-SQL](https://www.edx.org/course/querying-data-with-transact-sql-0)  
`` These queries works on AdventureWorks database and information regarding the same ``  
`` can be accessed by visiting following link - ``  
[AdventureWorks Installation and configuration](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017)
<hr />


In the last post, we started with Introduction to T-SQL, where some of the forms of <b>SELECT</b> statement were discussed, along with examples regarding <b> Data Type Conversion </b>, <b>Working with NULLs</b> and <b>CASE-END</b> operations were shown.  
You can follow the same - [Introduction to Transact-SQL - Part 1](https://aakashkh.github.io/tsql/2019/03/06/Introduction-To-TSQL.html)

We'll be discussing these in this post -
* [Removing Duplicates](#removing-duplicates)
* [Sorting Results](#sorting-results)
* [Paging Sorted Results](#paging-sorted-results)
* [Filtering Data](#filtering-data)

### Removing Duplicates
<b> Distinct </b> is used for getting results distinct values from the selected columns

* The following queries results in a column with distinct Color names with *null* replaced as *None*, creating a new category for such cases.

```sql
select
  distinct isnull(Color, 'None') as Color
from
  SalesLT.Product
```
### Sorting Results
<b> Order By </b> is used to order the final output return by sql queries by the order of the column in ascending / descending manner. The default order is ascending.
* The following queries will give the distinct color names sorted alphabetically.
<!--break-->

``` sql
select
  distinct isnull(Color, 'None') as Color
from
  SalesLT.Product
order by
  Color
```
* This one will do the same, sorting the output in alphabetical manner by column color of the output returned. Here we are bringing distinct values of color with replacing *null* by *None* in Color column and for each color bringing the size associated with them where *null* values are replaced by *-* character.

```sql
select
  distinct isnull(Color, 'None') as Color,
  isnull(Size, '-')
from
  SalesLT.Product
order by
  Color
```
### Paging Sorted Results
* Displaying top n rows - <b>Top N</b> command, where N is the no. of rows you want -

```sql
select
  top 10 Name,
  ListPrice
from
  SalesLT.Product
order by
  ProductNumber
```
* Displaying last n rows -  <b>Top N command with Order By Desc <b>, to get bottom N rows -

``` sql
select
  top 10 Name,
  ListPrice
from
  SalesLT.Product
order by
  ProductNumber Desc
```
* We can also get the percentage of data, by using <b>Top N percent</b> -

```sql
select
  top 10 percent Name,
  ListPrice
from
  SalesLT.Product
order by
  ProductNumber
```
* <b>Top N with ties</b> - Top N will always return you the top N rows, doesn't matter if they are duplicate or not. Whereas, Top N with ties will return all rows that have same values associated, will do it for all 10 distinct values -

<b>As per Microsoft documentation - </b> [With Ties](https://docs.microsoft.com/en-us/sql/t-sql/queries/top-transact-sql?view=sql-server-2017)  
``Return two or more rows that tie for last place in the limited results set.``
``You must use this argument with the ORDER BY clause. ``
``WITH TIES might cause more rows to be returned than the value specified in expression.``
``For example, if expression is set to 5 but two additional rows match the values of the ORDER BY columns in row 5,``
``the result set will contain seven rows.``  

```sql
-- return only top 10 rows
SELECT
  TOP 10 SalesOrderID, OrderQty
FROM
  SalesLT.SalesOrderDetail
ORDER BY
  OrderQty

-- return around 128 rows, all rows associated with where OrderQty is same is returned
SELECT
  TOP 10 with ties SalesOrderID, OrderQty
FROM
  SalesLT.SalesOrderDetail
ORDER BY
  OrderQty
```
* <b>Offset and Fetch</b> are used to implement a [query paging solutions](https://docs.microsoft.com/en-us/sql/t-sql/queries/select-order-by-clause-transact-sql?view=sql-server-2017#using-offset-and-fetch-to-limit-the-rows-returned)
  - Offset - no. of rows to ignore
  - Fetch Next - next set of rows to bring  

  The following query will bring rows no. 11-20 and not first 10 rows -

``` sql
-- fetch rows 11-20
select
  Name,
  ListPrice
from
  SalesLT.Product
order by
  ProductNumber offset 10 Rows Fetch Next 10 Rows Only

-- offset is 0, hence fetch first 10 rows
select
  Name,
  ListPrice
from
  SalesLT.Product
order by
  ProductNumber offset 0 Rows Fetch Next 10 Rows Only
```

### Filtering Data
Filtering data can be done using multiple ways - like, equal to, not equal to, between, and, in, like, is null, not null etc. Some of these can be implemented with multiple combinations as well. Following queries explore them in more details.
* Not equal to can be implemented by <b><></b> -

``` sql
-- Prodcut Id not equal to 6
Select
  Name,
  Color,
  Size
from
  SalesLT.Product
where
  ProductModelID <> 6
```
* <b>Is Not Null</b> - Return not null values only -

``` sql
Select
  Name
from
  SalesLT.Product
where
  SellEndDate Is Not Null;
```
* <b>Between and And </b> - both the ranges are inclusive -

``` sql
Select
  Name,
  SellEndDate
from
  SalesLT.Product
where
  SellEndDate Between '2006/1/1'
  and '2006/12/31'
```

* <b>In</b> - values matches in the particular list -

``` sql
-- Only those results where ProductCategoryID is 5,6 or 7
Select
  Name,
  ProductCategoryID
From
  SalesLT.Product
where
  ProductCategoryID in (5, 6, 7)
order by
  ProductCategoryID Desc

-- ProductCategoryID is 5, 6 or 7 and SellEndDate is not null
Select
  ProductCategoryID,
  Name,
  SellEndDate
From
  SalesLT.Product
where
  ProductCategoryID in (5, 6, 7)
  and SellEndDate Is Null
```

* <b>Like</b> is used to match string pattern -
  - <b> % </b> - anything string of 0 or more characters
  - <b> _ </b> - any single character
For more details - [Wildcard Characters](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-2017)

``` sql
-- ProductNumber starts with FR
select
  productnumber,
  Name,
  ListPrice
from
  SalesLT.Product
where
  ProductNumber like 'FR%'

-- ProductNumber starts with FR-,
-- then contains any single character followed by two single character in range 5-6 and 5-9 (both inclusive)
-- then conatins any single character followed by -
-- and finally any with two number ranging from 0-9
select
  Name,
  ListPrice,
  ProductNumber
from
  SalesLT.Product
where
  ProductNumber Like 'FR-_[5-6][5-9]_-[0-9][0-9]'

-- ProductNumber starts with FR or ProductCategoryID is 5,6,or 7
select
  Name,
  ProductCategoryID,
  ProductNumber
From
  SalesLT.Product
where
  ProductNumber like 'FR%'
  or ProductCategoryID IN (5, 6, 7)
```
<hr>
