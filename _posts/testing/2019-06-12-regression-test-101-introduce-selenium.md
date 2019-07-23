---
title: "Regression Test 101 - 1 - Selenium Introduction "  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

Recently I have been writing regression test for my current project. After the work, I think it's worthy to summarize what have done during the process. For myself, it's a reference for further work, I also hope it's a helpful reference to make the regression test more easier for other people. My project is a .net project, and we use **xUnit** and **Selenium webdriver(the C# implemantation)** to write regression test. So I will use xUnit and Selenium Webdriver for this series. But the concept and some solutions are applicable for other test framework and regression tools.

In this series, I will cover the following topics:


* [Selenium Introduction]({% link _posts/testing/2019-06-12-regression-test-101-introduce-selenium.md %})
* [Test Target]({% link _posts/testing/2019-07-15-regression-test-101-regression-test-target.md %})
* [Test Design]({% link _posts/testing/2019-07-16-regression-test-101-regression-test-design.md %})
* [Parrallel Execution]({% link _posts/testing/2019-07-17-regression-test-101-regression-parallel-execution.md %})
* [Test Information collection & Visualization]({% link _posts/testing/2019-07-22-regression-test-101-regression-test-visualization-information-collection.md %})
* [Challenges]({% link _posts/testing/2019-07-23-regression-test-101-regression-test-challenges.md %})


Let's start with the first topic.

### Selenium WebDriver

Selenium might be the most popular browser test tool. There are implementations in different languages, such as C#, java, python, ruby, etc. You can also test your website against different browsers, as there are many webdirver implementaions, such as ChromeDriver, FirfoxDriver, InternetExplorDriver, PhantomJSDriver, etc.

The webbriver includes page navigation, element locating and event trigger. The following is a code snipet from Selenium website[^1]:

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

### Integration with SpecFlow

SpecFlow is a member of Cucumber family -- a widly used Behavior Driven Development tool. It uses the Gherkin syntax(Given-When-Then) to create feature file and then hook it will C# implementaions. The Gherkin syntax makes the tests more readable.

A typical feature file looks like[^2]:

```gherkin
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

SpecFlow itself is not a UI test tool, but you can integrate it with Selenium WebDriver to do regression tests. 

In this series, we will not talk about how to write SpecFlow test. One reason is that the existing tests I am working on is written purely with Selenium WebDriver, the other reason is SpecFlow is not very strong related to this series. But I had worked on other projects with SpecFlow regression test, it's worthy to try it out.

