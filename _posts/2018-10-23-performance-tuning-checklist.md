---
title: Performance tuning Checklist
category: development  
tags: [sql, dot net, performance tuning, sql server, open xml sdk]  
layout: post  
lang: en
---

<div style='margin:0 auto;width:0px;height:0px;overflow:hidden;'>
<img src="/assets/images/performance-monitor.png" width='700'>
</div>

I did some performance tuning tasks recently, and summarized a checklist for performance tuning in the below:


## Checklist

### Before Start

These are the things you need to prepare or keep in mind before start working.

* Prepare same data size with Production database.
* Know about the user's behaviour, simulate user's operation to reproduce issue.
* Use tools to identify issue, do not make assumption.
* Validate often during tuning.
* Speed is not the only thing, please also check CPU, memory, Disk I/O, etc.
* Sometime, it is only because the server is not powerful enough. 


### Sever/System Related

It's about how to improve performance without changing code :)

* Upgrade CPU, memory, hard disk.
* Upgrade Database version, framework.
* Separate application into different servers. For example, put time consuming background tasks into a separate server.
* Load balancing for heavy traffic applications

### Database Related

SQL Server is the database that most of the projects I have worked on, but some points apply to other databases.(I am not an expert of Database, and these are the things that I usually check firstly).

* Check if index is missing, or need rebuild.
* Check execution plan to get more information
* Select just enough data, don't select unrelated data. 
* Beware the `Scalar-valued Function`. It hurts when handling huge data. There are `Scalar-valued Function`, `Inline Table-valued Function` and `Multi-Statement Table-valued Function`. Try to avoid the first and the third. Check [Here](https://www.mssqltips.com/sqlservertip/4772/refactor-sql-server-scalar-udf-to-inline-tvf-to-improve-performance/) and [Here](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-function-transact-sql?view=sql-server-2017) 
* Avoid N+1 Select
* COMPATIBILITY_LEVEL of SQL Server, sometimes it matters, check the [details](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-compatibility-level?view=sql-server-2017) 
* MAXDOP, usually you don't need to change this, or don't change it if you are not sure. I did found the instance MAXDOP set to 2 on one production database. Check [Here](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option?view=sql-server-2017) and [Here](https://sqltechblog.com/2016/10/04/understanding-the-new-maxdop-settings-in-sql-2016/) 
* ORM over Stored Procedure

### Front-End Code Related

There are some basic rules for good front-end performance, and you can easily get a list from google. 

There are also many front-end frameworks like angular, react, etc, and they have their own performance analysis tools.

Actually there are two very useful functions, which can record the timing.

```javascript
console.time('A');
console.timeEnd('A'); 
output> A: 9308.794921875ms
//The time is the interval between these two method calls.
```

I created a wrapper function:

```javascript
function timing(fn, parentName) {

    var name = new Date().toJSON() + "  ";
    if (parentName) {
        name = name + parentName + "-";
    }
    name = name + fn.name;

    return function () {
        console.time(name);
        const result = fn.apply(this, arguments);
        console.timeEnd(name);
        return result;
    };
};

// You can test like this:
// The name is combined by: [timestamp  ] + [parentName-] + [methodname] 
// Timestamp is used to avoid duplicated name
timing(await sleep)(1) 
output> 2018-10-26T11:59:43.402Z  sleep: 1001.0029296875ms
timing(await sleep, 'Second')(1)  
output> 2018-10-26T12:01:00.057Z  Second-sleep: 1000.207275390625ms

```

### Back-End Code Related

* use cache if needed. 
* Move expensive filters out of loop
* Avoid N+1(usually when executing DB query with ORM)
* Dictionary over List for item lookup.
* **Asynchronous and In Parallel (In a Non-Blocking Fashion)** for multiple time consuming queries in one request.


## A real-world performance tuning example

There is a download API, which is used to download data for some departments, and store the result in an excel file. We noticed that the api is slow even only for one department. It gets slower when there are more data in the result. It never finishes when downloading all departments data. Before we look into the issue, we thought it might be a database query performance issue, because there are lots of joins and calcuation in the sql query.

This is my local machine configuration:

| Property         | Value                      | 
|------------------|----------------------------| 
| Operation System | Windows 10 Pro             | 
| Memory           | 32G                        | 
| CPU              | i7 2.8GHz, 8 Core          | 
| Hard Disk        | 1T Toshiba SSD             | 
| Database         | SQL Server 2017            | 


### Step 1 - Identify bottle neck

* Use backup of production DB.
* Check the performance for different data size.
* Use `dotTrace` to get the time distribution.

There are 3 main steps in the API:

* **SQL**: Execute SQL scripts to load data from DB
* **C#**: Reorganize the result to match the Excel data format
* **Excel Writer**: Use Open XML SDK to write the data to Excel

Here is the timing for different queries:

| Sample No          | Excel Size | Total Time(ms) | SQL(ms) | C#(ms) | Excel Writer(ms) | 
|--------------------|------------|----------------|---------|--------|------------------| 
| Size 1(2019-1-001) | 4335 * AO  | 32336          | 20141   | 8913   | 3234             | 
| Size 2(2019-1-008) | 4148 * AL  | 25963          | 16033   | 7171   | 2733             | 
| Size 3(2019-1-012) | 6036 * CR  | 63652          | 17399   | `35465`| `10721`          | 
| Size 4(2019-1-ALL) | 93000 * HG | -              | 65065   | -      | -                | 


From the results, we can see:

* **SQL**  gets slow when data size increase, but not the bottle neck
* **C#** get worse dramatically when data size increase, need to solve
* **Excel Writer** get worse dramatically when data size increase, need to solve.

So the action is to fix the part of **C#** and **Excel Writer** first.

###  Step 2 - Fix `C#`

After looked into the code, we found the following bad smells:

* There are some reference data list used in several level nested loop, use dictionaries to index the data with the query, move them out of loops
* Extract some variable operation out of the loop, as the value don't change in the loop.

This is the result after this round of refactoring:

| Sample No          | Excel Size | Total Time(ms) | SQL(ms) | C#(ms)  | Excel Writer(ms) | 
|--------------------|------------|----------------|---------|---------|------------------| 
| Size 4(2019-1-ALL) | 93000 * HG | 435788         | 37205   | `12854` | `385061`         | 

This is amazing! We reduced the time dramatically by using Dictionary and moving codes out of loops. Now the download can be finished  in ~ 7 min.

### Step 3 - Fix `Excel Writer`

The library used for excel generation is `DocumentFormat.OpenXml.Spreadsheet`, and I found `cell.DataType = CellValues.InlineString;` costs lots of time, so I tried different ways:

| Set CellType By                                                      | Excel Writer(ms) | 
|----------------------------------------------------------------------|------------------| 
| cell.DataType = CellValues.InlineString                            | 385061           | 
| //Don't set                                                        | 150316           | 
| cell.SetAttribute(new OpenXmlAttribute("", "t", "", "inlineStr")); | 168932           | 

Based on the above result, I choosed the third approach.

But it still costs ~ 3 minutes, and at the same time I found the memory consumption is very high (~13 G). After further investigation, I found that there are two ways to write excel: **Open XML SDK DOM Approach** and **Open XML SDK SAX-Like Approach**. And it's said that the later consumes less memory[^1].

Here is the result for different approaches:

| Approach                           | Excel Writer(ms) | CPU  | Memory | 
|------------------------------------|------------------|------|--------| 
| **Open XML SDK DOM Approach**      | 168932           | >60% | ~13G   | 
| **Open XML SDK SAX-Like Approach** |  85478           | <30% | ~2G    | 


Now The Excel Writer only cost ~ 85s. The total download time is ~ 2 min, which is much better than before. After talked with PO, this is an acceptable result. 


### Result

Here is the comparison between the old codes and tuned ones for downloading all departments data:

| Measurement | Before                            | After | 
|-------------|-----------------------------------|-------| 
| Time        | Still not finished after one hour | 2 min | 
| CPU         | >90%                              | <30%  |
| Memory      | 13G                               | 2G    |

When look back the whole performance tuning process, we found that the issue is totally different with what we have thought at the beginning. By following the right process, we identified and fixed issue quickly.

[^1]: [https://blogs.msdn.microsoft.com/brian_jones/2010/06/22/writing-large-excel-files-with-the-open-xml-sdk/](https://blogs.msdn.microsoft.com/brian_jones/2010/06/22/writing-large-excel-files-with-the-open-xml-sdk/)


