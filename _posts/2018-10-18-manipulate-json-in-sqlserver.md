---
title: Manipulate JSON in SQL Server  
category: database  
tags: [json, sql server, nosql]  
layout: post  
lang: en  
---

Sql Server2016 starts to support JSON data operation. But you still need to make sure the `COMPATIBILITY_LEVEL` >= 130 in case your database is restored/migrated from an older version.

Please check [this link](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-compatibility-level?view=sql-server-2017) for the mapping of Sql Server version and Compatibility Level.

Check the compatilibity level:

```sql
select * from sys.databases
```

Update the value if your Sql Server supports:
```sql
ALTER DATABASE TestDb SET COMPATIBILITY_LEVEL = 130 -- if less than 130.
```

# Examples

Following is the sample data:

```sql
DECLARE @jsonVariable NVARCHAR(MAX)

SET @jsonVariable = N'[  
        {  
          "Order": {  
            "Number":"SO43659",  
            "Date":"2011-05-31T00:00:00"  
          },  
          "AccountNumber":"AW29825",  
          "Item": {  
            "Price":2024.9940,  
            "Quantity":1  
          }  
        },  
        {  
          "Order": {  
            "Number":"SO43661",  
            "Date":"2011-06-01T00:00:00"  
          },  
          "AccountNumber":"AW73565",  
          "Item": {  
            "Price":2024.9940,  
            "Quantity":3  
          }  
       }  
  ]'

select @jsonVariable as Orders
into SalesReportJson 
```

# Functions

## OPENJSON
You can also trasform the JSON Data, and save it to table

```sql
SELECT SalesOrderJsonData.* 
into SalesReport
FROM OPENJSON (@jsonVariable)  
           WITH (  
              Number   varchar(200) N'$.Order.Number',   
              Date     datetime     N'$.Order.Date',  
              Customer varchar(200) N'$.AccountNumber',   
              Quantity int          N'$.Item.Quantity'  
           )  
  AS SalesOrderJsonData; 
```

Or read from **SalesReportJson** table directly:

```sql
select Orders.* 
into SalesReport
from SalesReportJson
cross apply openjson(Orders) 
    WITH (  
        Number   varchar(200) N'$.Order.Number',   
        Date     datetime     N'$.Order.Date',  
        Customer varchar(200) N'$.AccountNumber',   
        Quantity int          N'$.Item.Quantity'  
    )  as Orders
  
```
The **SalesReport** table will look like:

| Number  | Date                    | Customer | Quantity |
| ------- | ----------------------- | -------- | -------- |
| SO43659 | 2011-05-31 00:00:00.000 | AW29825  | 1        |
| SO43661 | 2011-06-01 00:00:00.000 | AW73565  | 3        |


## ISJSON
It is used to check if the data is in a valid json format.

The following sql will return **1**:

```sql
select ISJSON(Orders) from SalesReportJson
```
## JSON_VALUE

**JSON_VALUE** is used to extract a scalar value from a JSON string. 
If the value is not a scalar value, the result will be `NULL`[^1]. In that case, you should use **JSON_QUERY** instead.

For example, if we want to get the first order's order number from the Orders column of **SalesReportJson**, run the following sql:

```sql
  select JSON_VALUE(Orders, '$[0].Order.Number') as OrderNumber,
         JSON_VALUE(Orders, '$[0].Item.Quantity') as ItemQuantity,
         JSON_VALUE(Orders, '$[0].Order') as FirstOrder -- this will be NULL 
  from SalesReportJson
```
The result will be:

| OrderNumber | ItemQuantity | FirstOrder |
| ----------- | ------------ | ---------- |
| SO43659     | 1            | NULL     |

[^1]: Actually there are two mode: `lax` and `strict`. it returns NULL in lax mode, but return Error in strict mode. Change the expression to `'strict $[0].Order'` to enable strict mode. The lax mode is the default.

## JSON_QUERY

JSON_QUERY is used to extract object or list from a JSON string. For the previous example, we can use the JSON_QUERY to get the FirstOrder object:

```sql
select JSON_QUERY(Orders, '$[0].Order') as FirstOrder -- this will be an object 
from SalesReportJson
```
The result will be :

| FirstOrder                                                                                      | 
|-------------------------------------------------------------------------------------------------| 
| {                "Number":"SO43659",                "Date":"2011-05-31T00:00:00"              } | 
 

## JSON_MODIFY

It updates the value of a property in a JSON string and returns the updated JSON string.

For example, we update the item price of the first order:

```sql
  update  SalesReportJson
  set Orders=JSON_MODIFY(Orders, '$[0].Item.Price', 100.0)
```

The result will be:
```json
[
    {
        "Order": {
            "Number": "SO43659",
            "Date": "2011-05-31T00:00:00"
        },
        "AccountNumber": "AW29825",
        "Item": {
            "Price": 100.0,
            "Quantity": 1
        }
    },
    {
        "Order": {
            "Number": "SO43661",
            "Date": "2011-06-01T00:00:00"
        },
        "AccountNumber": "AW73565",
        "Item": {
            "Price": 2024.9940,
            "Quantity": 3
        }
    }
]
```

## FOR JSON

with **FOR JSON**, You can also format query results as JSON.

```sql
  select Number as [Order.Number],
  Date as [Order.Date],
  Customer as [Account],
  Quantity as [Item.Quantity]
  from SalesReport
  FOR JSON PATH, ROOT('Orders')  -- use PATH here to make the result nested according to the dot syntax 
```

The result json is :

```json
{
    "Orders": [
        {
            "Order": {
                "Number": "SO436592",
                "Date": "2011-05-31T00:00:00"
            },
            "Account": "AW298252",
            "Item": {
                "Quantity": 12
            }
        },
        {
            "Order": {
                "Number": "SO43661",
                "Date": "2011-06-01T00:00:00"
            },
            "Account": "AW73565",
            "Item": {
                "Quantity": 3
            }
        }
    ]
}
```

The same sql but with AUTO mode:

```sql
  select Number as [Order.Number],
  Date as [Order.Date],
  Customer as [Account],
  Quantity as [Item.Quantity]
  from SalesReport
  FOR JSON AUTO, ROOT('Orders') -- auto mode
```

The result will be:

```json
{
    "Orders": [
        {
            "Order.Number": "SO436592",
            "Order.Date": "2011-05-31T00:00:00",
            "Account": "AW298252",
            "Item.Quantity": 12
        },
        {
            "Order.Number": "SO43661",
            "Order.Date": "2011-06-01T00:00:00",
            "Account": "AW73565",
            "Item.Quantity": 3
        }
    ]
}
```

When you join tables, columns in the first table are generated as properties of the root object. Columns in the second table are generated as properties of a nested object. The table name or alias of the second table is used as the name of the nested array. For example:

```sql
  select 123 as Id, 'Jason' as FirstName, 'json@gmail.com' as Email
  into Person

  select 123 as PersonId, '1/10 High street' as HomeAddress
  into Address

  select * from Person 
  join Address on Id = PersonId
  FOR JSON AUTO, Root('Users')
```

The result will be:

```json
{
    "Users": [
        {
            "Id": 123,
            "FirstName": "Jason",
            "Email": "json@gmail.com",
            "Address": [
                {
                    "PersonId": 123,
                    "HomeAddress": "1\/10 High street"
                }
            ]
        }
    ]
}
```

# More on Arrays

We can use index to locate the item in an array. Actually when we use OPEN_JSON to read the array in a JSON string, it will output some other informations:

```sql
select Order2s.*
from SalesReportJson
cross apply openjson(Orders)  as Order2s
```
The result will be: 

| key | value                                                                                                                                                                                                                                                                            | type | 
|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------| 
| 0   | {              "Order": {                "Number":"SO43659",                "Date":"2011-05-31T00:00:00"              },              "AccountNumber":"AW29825",              "Item": {                "Price":100.0,                "Quantity":1              }            }    | 5    | 
| 1   | {              "Order": {                "Number":"SO43661",                "Date":"2011-06-01T00:00:00"              },              "AccountNumber":"AW73565",              "Item": {                "Price":2024.9940,                "Quantity":3              }           } | 5    | 

The first column is `key`, which is the index of each item in the array, the column `type` indicate it's an object value. with the `key`, we can do some conditional modification to some items in an array.

