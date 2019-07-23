---
title: "Regression Test 101 - 6 - Challenges"  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

This is the last topic in the **regression test 101** series. In this series, I will cover the following topics:

* [Selenium Introduction]({% link _posts/testing/2019-06-12-regression-test-101-introduce-selenium.md %})
* [Test Target]({% link _posts/testing/2019-07-15-regression-test-101-regression-test-target.md %})
* [Test Design]({% link _posts/testing/2019-07-16-regression-test-101-regression-test-design.md %})
* [Parrallel Execution]({% link _posts/testing/2019-07-17-regression-test-101-regression-parallel-execution.md %})
* [Test Information collection & Visualization]({% link _posts/testing/2019-07-22-regression-test-101-regression-test-visualization-information-collection.md %})
* [Challenges]({% link _posts/testing/2019-07-23-regression-test-101-regression-test-challenges.md %})

During implementing the regression tests, I encountered some challenges, some of them already covered in the previous articles, but I still summarized them here for a reference.

#### 1. File Upload dialog

When running test parallely on windows, the file upload dialog will break the tests, as selenium itself has no control on the system upload file dialog. There are some libraries/tools which can manipulate the dialog, but they are probably not designed for multi-threads. The solution is covered in [Parrallel Execution]({% link _posts/testing/2019-07-17-regression-test-101-regression-parallel-execution.md %})


#### 2. Capture screenshot in EventFiringWebDriver's ExceptionThrown delegate

If you have some code to wait for some element to show or clickable, you may need the `WebDriverWait` class. The code may look like:

```csharp
var webDriverWait = new WebDriverWait(Driver, TimeSpan.FromSeconds(60))
webDriverWait.Until(d => d.FindElement(locator));
```

In the C# implementation, The  `FindElement` method will throw exception if the element can't be located, which will trigger the EventFiringWebDriver's ExceptionThrown delegate. If you don't distinguish the excetion types, you may got a huge mount of screenshots.

The other issue is that `WebDriverWait` will throw exception after the timeout duration, but the exception here is not able to be captured by WebDriver's ExceptionThrown handler. If you want to capture the screenshot for this exception,you need also add the ScreenshotRecorder's TakeScreenshot call here.

Some people may just capture screenshot for all exceptions by put the test code into a `try` `catch` block, but in this case, you need to check if the driver is available.


#### 3. Remove data dependencies between tests.

One easy way to make sure the test always running on new data is to use unique identifier(for example Guid) for the data. For example the test creates user during test, you may assign `User-{Guid.NewGuid()}` as the user name. Beware of the length limitation on the user name columns, if Guid value is too long,  datetime string(`yyyyMMddTHHmmss`) would be a nice candidate.

#### 4. Alert in Browser

Sometimes there will be alert popup, which will fail the following test if you don't deal with it. A typical way is to handle it before test run:

```csharp
try
{
    driver
        .SwitchTo()
        .Alert()
        .Accept();
}
catch (Exception)
{
    // No alert displayed
}
```

Please remember the `Alert()` and `Accept()` methods may also throw exceptions, which trigger the `ExceptionThrown` handler if you are using `EventFiringWebDriver`.


#### 5. Cavas in HTML

Selenium can't handle the `Cavas` content in html easily. You may be able to use `Actions` provided by Selenium to control mouse movement to finish some simple operations, but it's still not easy to write. As in my project we only use cavas in one place, I just ignored the test for canvas content.
