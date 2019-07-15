---
title: "Regression Test 101 - Intro"  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

Recently I have been writing regression test for my current project. Before I started, there were already many regression tests in the code base, but it's not been maintained for a long time, and the project structure and database changed a lot during the time. My task is to make it run again. 

It seems an easy task, but after I started working on it, I realized that there are more work to do, otherwise, it may be broken and discarded sooner or later. In the following I will talk about some common questions about regression test, and how to solve them. My project is a .net project, and we use xUnit and Selenium webdriver to write regression test. But the solution/concept can also applied to other frameworks.


## Tool selection
SpecFlow, Selenium


You don't need to write C# code for regression test, as it's UI test. But if you really want to use C#, there are two options: Selenium Webdriver and SpecFlow.

### Selenium WebDriver

Selenium might be the most popular browser test tool. There are implementations in many languages, such as C#, java, python, ruby, etc.
There are also many webdirver implementaions, such as ChromeDriver, FirfoxDriver, InternetExplorDriver, etc.

The following is a code snipet from Selenium website[^1]:

```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Firefox;

// Requires reference to WebDriver.Support.dll
using OpenQA.Selenium.Support.UI;

class GoogleSuggest
{
    static void Main(string[] args)
    {
        // Create a new instance of the Firefox driver.
        // Note that it is wrapped in a using clause so that the browser is closed 
        // and the webdriver is disposed (even in the face of exceptions).

        // Also note that the remainder of the code relies on the interface, 
        // not the implementation.

        // Further note that other drivers (InternetExplorerDriver,
        // ChromeDriver, etc.) will require further configuration 
        // before this example will work. See the wiki pages for the
        // individual drivers at http://code.google.com/p/selenium/wiki
        // for further information.
        using (IWebDriver driver = new FirefoxDriver())
        {
            //Notice navigation is slightly different than the Java version
            //This is because 'get' is a keyword in C#
            driver.Navigate().GoToUrl("http://www.google.com/");
    
            // Find the text input element by its name
            IWebElement query = driver.FindElement(By.Name("q"));
    
            // Enter something to search for
            query.SendKeys("Cheese");
    
            // Now submit the form. WebDriver will find the form for us from the element
            query.Submit();
    
            // Google's search is rendered dynamically with JavaScript.
            // Wait for the page to load, timeout after 10 seconds
            var wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
            wait.Until(d => d.Title.StartsWith("cheese", StringComparison.OrdinalIgnoreCase));
    
            // Should see: "Cheese - Google Search" (for an English locale)
            Console.WriteLine("Page title is: " + driver.Title);
        }
    }
}
```

[^1]: https://www.seleniumhq.org/docs/03_webdriver.jsp#introducing-the-selenium-webdriver-api-by-example

### SpecFlow

SpecFlow is a member of Cucumber family - a widly used Behavior Driven Development tool. It uses the Gherkin syntax(Given-When-Then) to create  feature file and then hook it will C# implementaions. The Gherkin syntax makes the tests more readable.

As SpecFlow is a Behavior Driven Development tool, it's not limited only to UI test.


A typical feature file looks like[^2]:

```
Feature: Calculator
       In order to avoid silly mistakes
       As a math idiot
       I want to be told the sum of two numbers

@mytag
Scenario: Add two numbers
       Given I have entered 50 into the calculator
       And I have also entered 70 into the calculator
       When I press add
       Then the result should be 120 on the screen
```

[^2]: https://specflow.org/getting-started/

And the c# implementaion is[^2]: 

![image](/assets/images/specflow-example.png)

We have used SpecFlow, Cucumber, Selenium in several different c# projects. 

## Data Setup/Clean up
Create new data for every run. Clean database after run, or restore to a baseline.
## Test Infrastructure
Pages definition, Page section definition
## Test Run Order & Parrallel Excution
Test Collection, Test Priority, Feature(group tests together)
web application performance,
CI build agent performance,
dependencies(between tests, system related(file upload dialog))
## Info collection
page loading time, page navigations, screenshots(selenium level, test failure).
## Reporting
Failures, Passed, ratio, duration, test coverage.
