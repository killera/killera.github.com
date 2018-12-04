---
title: "Everyday SQL - How to delete duplicated rows"  
category: development  
tags: [database, sql, sql server, rank, dense_rank, row_number]  
layout: post  
lang: en  

---

Today we are going to delete the duplicated rows in the database.
We have a table to store the N-N relationship between order and Product, which contains three columns: `Id` for PK, `OrderId` and `ProductId`. There is no unique constraint on column combination: [`OrderId`, `ProductId`]. The data is as follow:

```sql
WITH T(Id, OrderId, ProductId)
     AS (SELECT 1,1,1 UNION ALL
         SELECT 2,1,1 UNION ALL
         SELECT 3,1,1 UNION ALL
         SELECT 4,1,2)
SELECT * from T
```

If you are not familiar with the `WITH`[^1] clause, it doesn't matter. You can consider that there is a table `T` contains the above data.

The data looks like: 

| Id | OrderId | ProductId | 
|----|---------|-----------| 
| 1  | 1       | 1         | 
| 2  | 1       | 1         | 
| 3  | 1       | 1         | 
| 4  | 1       | 2         | 

if we consider value pair (OrderId, ProductId) as unique, then Row 1, 2 and 3 are duplicated rows, we need to delete either two of them.

There are several functions for ranking in SQL Server: ROW_NUMBER, RANK, DENSE_RANK, NTILE. 


## Which to use? ROW_NUMBER, RANK, or DENSE_RANK?


The syntaxes of these three are similiar:

 ```sql 
ROW_NUMBER() | DENSE_RANK() | RANK()  
OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )
```

** The `PARTITION BY` clause will decide what rows will be grouped into same partition, and the `order_by_clause` will affect the number(row number or rank) for each row in the same partition. For the number, the ROW_NUMBER() always returns sequential numbers(for example 1, 2, 3, 4, 5), RANK() returns the same numeric value for ties(for example 1, 2, 2, 4, 5), while DENSE_RANK() returns numberic values without gaps(for example 1, 2, 2, 3, 4).

That's the different between those three functions.

### Use ROW_NUMBER() to identify duplication

Let's try `ROW_NUMBER` first. The usage of `ROW_NUMBER` is:

In our case, [OrderId, ProductId] is used for duplication check, and we want to keep the id with max value in each partition. So our sql will look like:

```sql
WITH T(Id, OrderId, ProductId)
     AS (SELECT 1,1,1 UNION ALL
         SELECT 2,1,1 UNION ALL
         SELECT 3,1,1 UNION ALL
         SELECT 4,1,2)

SELECT *,
	   ROW_NUMBER() OVER(PARTITION BY OrderId, ProductId ORDER BY Id DESC) AS 'ROW_NUMBER'
 FROM T
```

The result is: 

| Id | OrderId | ProductId | ROW_NUMBER | 
|----|---------|-----------|------------| 
| 3  | 1       | 1         | 1          | 
| 2  | 1       | 1         | 2          | 
| 1  | 1       | 1         | 3          | 
| 4  | 1       | 2         | 1          | 


In the result we can see the first three rows are with row number 1, 2, 3, and Id with 3 has the lowest row number 1. Then we can delete all rows the row number of which is not 1.


### Try RANK() and DENSE_RANK()

As we mentioned above, Use `RANK` or `DENSE_RANK` will return the same result:

```sql
WITH T(Id, OrderId, ProductId)
     AS (SELECT 1,1,1 UNION ALL
         SELECT 2,1,1 UNION ALL
         SELECT 3,1,1 UNION ALL
         SELECT 4,1,2)

SELECT *,
	   ROW_NUMBER() OVER(PARTITION BY OrderId, ProductId ORDER BY ProductId DESC) AS 'ROW_NUMBER',
	   RANK() OVER(PARTITION BY OrderId, ProductId ORDER BY Id DESC) AS 'RANK',
	   DENSE_RANK() OVER(PARTITION BY OrderId, ProductId ORDER BY Id DESC) AS 'DENSE_RANK'
 FROM T
```


The result is the same as ROW_NUMBER:

| Id | OrderId | ProductId | ROW_NUMBER | RANK | DENSE_RANK | 
|----|---------|-----------|------------|------|------------| 
| 3  | 1       | 1         | 1          | 1    | 1          | 
| 2  | 1       | 1         | 2          | 2    | 2          | 
| 1  | 1       | 1         | 3          | 3    | 3          | 
| 4  | 1       | 2         | 1          | 1    | 1          | 


If we change the `ORDER BY` clause, the `RANK` and `DENSE_RANK` will return different results. Here we use `Id % 2` in the `ORDER BY` clause to return 0 for even Id and 1 for odd Id. See how it affects the rank.

```sql
WITH T(Id, OrderId, ProductId)
     AS (SELECT 1,1,1 UNION ALL
         SELECT 2,1,1 UNION ALL
         SELECT 3,1,1 UNION ALL
         SELECT 4,1,2)

SELECT *,
	   RANK() OVER(PARTITION BY OrderId, ProductId ORDER BY Id%2 DESC) AS 'RANK',
	   DENSE_RANK() OVER(PARTITION BY OrderId, ProductId ORDER BY Id%2 DESC) AS 'DENSE_RANK'
 FROM T
```

| Id | OrderId | ProductId | RANK | DENSE_RANK | 
|----|---------|-----------|------|------------| 
| 3  | 1       | 1         | 1    | 1          | 
| 1  | 1       | 1         | 1    | 1          | 
| 2  | 1       | 1         | 3    | 2          | 
| 4  | 1       | 2         | 1    | 1          | 


[^1]: To keep it simple, it's not allowed to have more than 1 `SELECT` clause followed. And it's just a query, we will have it in each code snippet.