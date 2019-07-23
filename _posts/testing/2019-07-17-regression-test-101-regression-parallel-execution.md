---
title: "Regression Test 101 - 4 - Parrallel Execution"  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

This is one topic in the **regression test 101** series. In this series, I will cover the following topics:

* [Selenium Introduction]({% link _posts/testing/2019-06-12-regression-test-101-introduce-selenium.md %})
* Test Target
* Test Design
* Parrallel Execution
* Test Information collection & Visualization

Today's topic is **Parrallel Execution**.

Parallel Execution is a good but also a challenge thing for UI test. UI tests are usually slow, if we can parallel run the test, this can save lots of time.

### Parallel Options

#### 1. Selenium-Grid

Selenium-Grid[^1] is one option to accelerate the regression test. Selenium-Grid allows you to run test on different machines, and it also support distributed test execution.

#### 2. Parallel by Test Framework

The other option is to utilize the test framework's parallel execution ability. We are using xUnit in the project, and xUnit support to run tests in multi-threads mode. 

There are three parallel options:

* `collections` - Parallelizes collections
* `assemblies` - Parallelizes assemblies
* `all` - Parallelizes both assemblies and collections.

There is another option `maxthreads` to control how many threads to use per assembly.

As there are different ways to run test, the options name might be different, please check the document for details.

In the project, I splitted the tests into several `collections`, and run them parallelly by collections.

### Prerequisitions
No mater which parallel option to select, we always need to meet the following requirements:

#### 1. No dependencies between tests
   
Having dependencies between tests makes the test difficult to maintain, and it also restricts that the tests have to be run in the same threads. With more and more tests added, the dependencies may become difficult to find and maintain.

If the test A depends some data from test B, one way is to prepare the data in the database initialization period. The other way is to reuse the test B's code to generate the data on the fly. I prefer the first option. as it's more efficient, and more easier to find out what data prepared by looking at the data initializaion codes.

#### 2. Work on different dataset for different tests

Another things need to consider is to let the (paralleled) tests running against different dataset. Sometimes, we may prepare some background data for test. Take a online shopping website, we may need to (parallely) test the inventory control and shopping cart against two different products. 

#### 3. Remove dependencies from system

This happens when you are testing some features which require the system resources. One example is the file upload. On windows, after you clicked the "Upload" button, a system upload dialog popped up to let you choose the files. Selenium webdriver has no control on the system dialogs. Even there are some tools/libraries that you can control the dialog, but teay are probably not designed for multi-threads. 

One solution is to put all these kind of test into one collection, which means run sequentially; the other solution is to remove the dependency from system. Selenium Webdriver has the ability to upload file without opening the system dialog, you can checkout from [here](https://sqa.stackexchange.com/questions/12851/how-can-i-work-with-file-uploads-during-a-webdriver-test)

#### 4. The application performance

Run regression test parallely may consume lots of system resources, and also may take down the application's performance. Do not make it load test. You may need:

* Run regression test based on a small database size, as more data means more loading/saving time.
* Choose the right threads count for different environemnts, if the CI agent is powerful enough, try a bigger threads counts, vise versa.
* 