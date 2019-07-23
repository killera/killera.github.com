---
title: "Regression Test 101 - 5 - Information Collection & Visualization"  
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


Today's topic is **Information collection & Visualization**.

### Visualization

There is not to much to talk for this part. All popular CI tools already have the test report functionality, and test framemworks/tools also have the ability to generate the report in different format. In my project, we are using Azure Devops,which have a dashboard for the projects, we put the test report widget in it to report the test result and trends.

### Information Collection

#### 1. Screenshots

It's very helpful to capture a screenshot when error happens, so that we can easily to identify the issue.

Selenium has a EventFiringWebDriver, which has some before* and after* event handlers. It also has a ExceptionThrown delegate, which can be used to capture screenshot. 

You can wrap your webdriver with EventFiringWebDriver. The code may looks like:

```csharp

var driver = new ChromeDriver();
var eventFiringWebDriver = new EventFiringWebDriver(driver);
eventFiringWebDriver.ExceptionThrown += TakeScreenShot;

...

public void TakeScreenShot(object sender, WebDriverExceptionEventArgs e)
{
    var recorder = new ScreenshotRecorder(e.Driver);
    recorder.TakeScreenShot(e.ThrownException);
}

```

The implementation of `TakeScreenShot` is not easy as its name. There are some traps I will covered in the next article.

Please remember that the exception here is thrown by the webdriver, if you want to capture other exceptions, you need to write some codes to cover it.

#### 2. Page Loading Time

As mentioned above, `EventFiringWebDriver` has some before* and after* event handlers, which can be used to capture some data like page loading time. You can use this as a application performance measure.

#### 3.  Test Parallel Execution Time 

This is useful for parallel test by collections. You can add duration check for each collection, then use the durations as a reference to make the time for different collections evenly. For example, if you got the following output:

```bash
Test ran 00:03:56.4176514 for collection 1
Test ran 00:03:59.2635600 for collection 2
Test ran 00:05:06.2575782 for collection 3 
Test ran 00:03:15.6212037 for collection 4
```

You will probably add a new test into collection 4.
