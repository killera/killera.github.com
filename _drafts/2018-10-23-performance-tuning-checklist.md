---
title: Performance tuning Checklist
category: development  
tags: [sql, dot net, performance tuning, sql server, open xml sdk]  
layout: post  
lang: en
---


I and my team members did some performance tuning tasks recently. I summarized a checklist for performance tuning. I will add more into this checklist once I have some new thoughts.  

## CheckList

### Before start

* Use similiar scale data
* Know about the user's behaviour
* Use correct tools to locate the bottleneck/issue, don't make assumption.
* Validate often
* Use performance monitor(CPU, memory)


### Sever related

* Upgrade Database, framework, or operation system
* Upgrade CUP, memory, hard disk
* Put time consuming background tasks into separate server, make sure the main web application won't be affected by them.


### Database Related

* Select just enough data only
* Beware the Scalar Function
* Avoid N+1 Select
* COMPATIBILITY_LEVEL


### Code Related

* Move expensive filters out of loop
* Dictionary over List 
* compare string.toupper() as dictioanry key, or make dictionary stringcompare.IgnoreCase


## An Example

#### System configuration:

| Property         | Value                      | 
|------------------|----------------------------| 
| Operation System | Windows 10 Pro             | 
| Memory           | 32G                        | 
| CPU              | i7 2.8GHz, 8 Core          | 
| Hard Disk        | 1T Toshiba SSD             | 
| Database         | Sql Server 2017            | 



#### Downloaded excel file size

| Data volume      | Value                      | 
|------------------|----------------------------| 
| Excel Size       | 59M                        | 
| Excel row * col  | ~93000*HG = 93000*(8*26+8) | 

#### Refactoring Result

| Measurement | Before                            | After | 
|-------------|-----------------------------------|-------| 
| CPU         | >60%                              | <30%  | 
| Memory      | 13G                               | 2G    | 
| Time        | Still not finished after one hour | 2 min | 



### Step 1 
* Use backup of production database.
* Select a typical filter.
* Check the performance for different data size.

#### Use different dataset to test 

| Sample No          | ExcelSize  | Total Time(ms) | SQL(ms) | C#(ms) | Excel Writer(ms) | 
|--------------------|------------|----------------|---------|--------|------------------| 
| Size 1(2019-1-001) | 4335 * AO  | 32336          | 20141   | 8913   | 3234             | 
| Size 2(2019-1-008) | 4148 * AL  | 25963          | 16033   | 7171   | 2733             | 
| Size 3(2019-1-012) | 6036 * CR  | 63652          | 17399   | 35465  | 10721            | 
| Size 4(2019-1-ALL) | 93000 * HG | -              | 65065   | -      | -                | 


#### Find the bottleneck
From the results, we can see:
* **SQL**  gets slow when data size increase, but not the bottle neck
* **C#** get worse dramatically when data size increase, need to solve
* **Excel Writer** get worse dramatically when data size increase, need to solve.

#### Actions

##### 1. **C#**

* There are some reference data list used in serveral level nested loop, use dictionaries to index the data with the query, move them out of loops
* Use StringComparer.IgnoreCaseInvariant when creating dictionaries.
* Extract some variable operation out of the loop, as the value don't change in the loop.

After this refactoring:

| Sample No          | ExcelSize  | Total Time(ms) | SQL(ms) | C#(ms) | Excel Writer(ms) | 
|--------------------|------------|----------------|---------|--------|------------------| 
| Size 4(2019-1-ALL) | 93000 * HG | 435788         | 37205   | 12854  | 385061           | 

##### 2. **Excel Writer**

The library used is `DocumentFormat.OpenXml.Spreadsheet`, and found ` cell.DataType = CellValues.InlineString;` costs lots of time, try different ways:

| Set CellType By                                                      | Excel Writer(ms) | 
|----------------------------------------------------------------------|------------------| 
| `cell.DataType = CellValues.InlineString`                            | 385061           | 
| `//Don't set `                                                       | 150316           | 
| `cell.SetAttribute(new OpenXmlAttribute("", "t", "", "inlineStr"));` | 168932           | 

So choose the third way, as there are still some cell type is number.

But it still cost several minutes, and we found the memory comsumption is very high(~13 G). Continue refactoring, and found there are two ways to write excel: **Open XML SDK DOM Approach** and **Open XML SDK SAX-Like Approach**. And it's said the later consumes less memory[^1].

Here is the result:

| Approach                           | Excel Writer(ms) | CPU  | Memory | 
|------------------------------------|------------------|------|--------| 
| **Open XML SDK DOM Approach**      | 168932           | >60% | ~13G   | 
| **Open XML SDK SAX-Like Approach** |  85478           | <30% | ~2G    | 

And the total time is 2 minutes.



[^1]: [https://blogs.msdn.microsoft.com/brian_jones/2010/06/22/writing-large-excel-files-with-the-open-xml-sdk/](https://blogs.msdn.microsoft.com/brian_jones/2010/06/22/writing-large-excel-files-with-the-open-xml-sdk/)


