---
title: Pains and Gains using DelegateDecompiler  
category: dotnet  
tags: [dotnet, entity framework]  
layout: post  
lang: en
---


*DelegateDecompiler* is a tool to decompile a delegate or method body to its lambda representaion. 

## Gains

It's very handy when you write Entity framework LINQ expressions.

For example, if you have a table `User` with two columns: `FirstName` and `LastName`:

```csharp

public class User{
    public string FirstName {get; set;}
    public string LastName {get; set;}

    [Computed]
    public string FullName => FirstName + " " + LastName; 
}

```

You can select users with a specified string in his Name by:

```csharp

var name = "John";
var result = await DBContext.Users.Select(x => x.FullName.Contains(name)).DecompileAsync().ToListAsync();
//Or
var result = DBContext.Users.Select(x => x.FullName.Contains(name)).Decompile().ToList();

```

Without DelegateDecompiler, you can't use FullName in the LINQ directly.

## Pains

But I found wired behaviour when querying a entity with navigation properties. This is an example:

This one will not load the `Customers` eagerly: 

```csharp

var program = DbContext.Programs
                .Decompile()
                .Include(x=>x.Discounts.Select(b=>b.Customers.Select(c=>c.Customer)))
                .FirstOrDefault(x => x.Name.Contains("Jan"));

```

This one does:

```csharp

var program = DbContext.Programs
                .Include(x=>x.Discounts.Select(b=>b.Customers.Select(c=>c.Customer)))
                .Decompile()
                .FirstOrDefault(x => x.Name.Contains("Jan"));

```

As you may have noticed, in the second code snipet. the `Decompile()` method is placed just before the last query.

Especially, if you disabled the *Lazy Loading* in Entity framework, the `Customers` property of `Discount` will be null. This is not acceptable.



So, remember always put `Decompile()` or `DecompileAsync()` at the end of the LINQ chain.