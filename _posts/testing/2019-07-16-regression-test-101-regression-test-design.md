---
title: "Regression Test 101 - 3 - Test Design"  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

This is one topic in the **regression test 101** series. In this series, I will cover the following topics:


* [Selenium Introduction]({% link _posts/testing/2019-06-12-regression-test-101-introduce-selenium.md %})
* [Test Target]({% link _posts/testing/2019-07-15-regression-test-101-regression-test-target.md %})
* [Test Design]({% link _posts/testing/2019-07-16-regression-test-101-regression-test-design.md %})
* [Parrallel Execution]({% link _posts/testing/2019-07-17-regression-test-101-regression-parallel-execution.md %})
* [Test Information collection & Visualization]({% link _posts/testing/2019-07-22-regression-test-101-regression-test-visualization-information-collection.md %})
* [Challenges]({% link _posts/testing/2019-07-23-regression-test-101-regression-test-challenges.md %})



Today's topic is **Test Design**.

Before writing test, we need to think about how the test code will be organized. There are several general things need to be considered.

## Page Object Model

It's easy to use Selenium WebDriver to navigate between pages, or do some operation on pages. If we look back to the selenium code on the first blog [Selenium Introduction]({% link _posts/testing/2019-06-12-regression-test-101-introduce-selenium.md %}), there are usually serveral opertions on the pages - **navigation**, **element locating**, **setting values**, **triggering events**. This can become very difficult to maintain if not organized properly. Selenium suggest to use Page Object Model[^1] to define and organize the page related codes.

For example, we can define a LoginPage, with a login sumit form defined(a username element, a password element, and a submit button.) Here is the code:

```csharp
public class LoginPage {

    public LoginPage(IWebDriver driver){
         _driver = driver;
         _username = _driver.FindElement(By.Id("username"));
         _password = _driver.FindElement(By.Id("password"));
         _loginButton = _driver.FindElement(By.Id("submit-button"))
    }

    private IWebDriver _driver;
    private IWebElement _username;
    private IWebElement _password;
    private IWebElement _loginButton;
    
    public Login(string userName, string password){
        _username.SendKeys(userName);
        _password.SendKeys(password);
        _loginButton.Click();
    }
}
```

## Element Component and Page Section.

Sometimes there are some pages having the same section/component, such as, dropdown list, file upload, list filtering, navigation bar. For these elements, it will save lots of time to have a common section definition to encapsulate the logic. 

For example, if there is a tab navigation for several pages, we can define a `NavigationSection` object.

```csharp
public class PageNavigationActionSection : IPageSection
{
    public PageNavigationActionSection(IWebDriver driver)
    {
        _driver = driver;
    }
    private IWebDriver _driver;

    public void NavigateTo(string menuItem) => 
                _driver.Click(By.XPath($"//div[@class=\'tab\']//a[.=\'{menuItem}\']"));
}

public class DetailsPage
{
    public PageNavigationActionSection Navigator;
}
```

## Collection - Test Grouping

In xUnit, there is a feature **Collection Fixtures**, which is used to share test context between test classes. For example,all tests share the same database initialization. Another example is to use **Collection Fixtures** to group tests and make them run parallely by collections. About **Parallel Execution**, we will discussed in the following [topic]({% link _posts/testing/2019-07-17-regression-test-101-regression-parallel-execution.md %}).

## Data Setup/Clean up

As said above, the test case can share the data preparation code for reference data. But for each test, it shouldn't rely on other tests' data. So it's a good practice to follow the next rules:

* All tests share same reference data initializaion.
* The test should always create new data, shouldn't reuse the data generated in a previous run.
* The data should be cleanup at certain time.

After followed the above structure, our test structure will look like below:
![image](/assets/images/test-structure.png)


[^1]: https://www.seleniumhq.org/docs/06_test_design_considerations.jsp#page-object-design-pattern