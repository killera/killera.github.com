---
title: ".Net Core - entity framework LINQ evaluation, did you do it wrong?"  
category: dotnetcore  
tags: [entity framework core, sql, sql server, dotnetcore, linq]  
layout: post  
lang: en  

---

## EF 6 vs. EF Core

In a previous blog [Pains and Gains using DelegateDecompiler](/dotnet/2017/03/03/pains-and-gains-with-DelegateDecompiler), I mentioned a library `DelegateDecompiler` which can used to decompile method body when you use LINQ to query data in EF 6. Is there anything changed in EF Core[^1]?  Let's have a look.

We have the following data in table `Person` of database TestDB.


| Id  | FirstName | Email          | LastName   | 
|-----|-----------|----------------|------------| 
| 123 | Jason     | json@gmail.com | Smith      | 
| 124 | Dick      | dick@gmail.com | Smith      | 

This is the C# code:

```csharp
// DbContext
public class TestDbContext : DbContext
{
    public TestDbContext(DbContextOptions options) : base(options) { }
    public DbSet<Person> Persons { get; set; }
}

//Entity
[Table("Person")]
public class Person
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }

    [Computed]
    [NotMapped]
    public virtual string FullName => FirstName + " " + LastName;
}

//Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    ... //other codes

    services.AddDbContext<TestDbContext>(x => x.UseSqlServer(Configuration.GetConnectionString("TestDb")) );

    ... //other codes
}

//
```

Then in the `HomeController`, we use `TestDbContext` to fetch use data: 

```csharp
public class HomeController : Controller
{
    private readonly TestDbContext _dbContext;

    public HomeController(TestDbContext dbContext )
    {
        _dbContext = dbContext;
    }
    public IActionResult Index()
    {
        var persons = _dbContext.Persons
            .Where(x => x.FullName.Contains("Jason"))
            .ToList();

        return View(persons);
    }
}
```
We didn't use DelegateDecompiler here. If we check the result, we can see that it returned exactly the record we want. It seems that EF Core can handle the expression evaluation now. Maybe it's time to remove all DelegateDecompiler dependencies in the code!

**But, wait..** 

Let's check the generated SQL first. We will get the following query from SQL Profiler:

```sql
SELECT [x].[Id], [x].[Email], [x].[FirstName], [x].[LastName]
FROM [Person] AS [x]
```

What!? It ignored the filter and returned all the data!! 

Yes, EF Core introduced a new feature called "Client Evaluation". In the above example, as EF Core is not able to evaluation `FullName` Property, it fetches all the data back, and then evaluates the `Where` clause in the code. We will discuss this feature in the next.

## Server vs. Client Evaluation

From Microsoft documentation, there is one page **Server vs. Client Evaluation**[^2] talking about this.

> Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database. It is up to the database provider to determine which parts of the query will be evaluated in the database.

It's useful in some cases, but it can also results in a poor performance in some other cases. EF Core will log warning when the client evaluation is performed, you can check it to see if client evaluation happens.

If you don't want EF Core do client evaluation silently, you can configure it to throw exception when it happens, and leave the job to `DelegateDecompiler`:

```csharp
//Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    ... //other codes

    services.AddDbContext<TestDbContext>(x => x.UseSqlServer(Configuration.GetConnectionString("TestDb"))
            .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning)));

    ... //other codes
}
```

## Further Actions

I don't want the code executed differently with what it looks like, so I prefer to set the db context options to throw exception, and leave the job to `DelegateDecompiler`.

EF Core does have some plan for this, in the comming EF Core 3.0[^3] release road map, it mentioned that there will be some improvement to make EF Core translate more expressions correctly and generate efficient queries in more cases. Let's have a look then. 

It also mentioned that, the  EF Core 3.0 Preview 1 packages doesn't include any new features yet.


Another thing to mention is that, both EF Core and `DelegateDecompiler` can not handle `string.Format()` or string interpolation[^4]. So the following code does not work as expected:

```csharp
public virtual string FullName => $"{FirstName} {LastName}";
// Or
public virtual string FullName => string.Format("{0} {1}", FirstName, LastName);
```


## References

[^1]: The version of EF Core used here is `2.1.1`

[^2]: [https://docs.microsoft.com/en-us/ef/core/querying/client-eval](https://docs.microsoft.com/en-us/ef/core/querying/client-eval)

[^3]: [https://docs.microsoft.com/en-us/ef/core/what-is-new/roadmap#ef-core-30](https://docs.microsoft.com/en-us/ef/core/what-is-new/roadmap#ef-core-30)

[^4]: [https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated)

