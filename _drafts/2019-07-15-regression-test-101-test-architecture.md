---
title: "Regression Test 101 - 2 - Test Architecture"  
category: development  
tags: [ui test, selenium, web driver, regression test, smoke test]  
layout: post  
lang: en  

---

This is the ** topic in the regression test 101 series. In this series, I will cover the following topics:

* [Selenium Introduction]({% link _drafts/2019-06-12-regression-test-101-introduce-selenium.md %})
* Regresion Test Target
* Test Architecture
* Data Setup/Clean up
* Parrallel Execution
* Test Information Collection
* Reporting and Visualization

Today's topic is **Test Architecture**.

Before writing test, we need to think about how the test code will be organized. There are several general things need to be considered.

## Page Object Model

It's easy to use Selenium WebDriver to navigate and do operations on the pages. Look back to the selenium code on [Selenium Introduction]({% link _drafts/2019-06-12-regression-test-101-introduce-selenium.md %}), there are usually serveral opertions on the pages - navigation, element locating, filling values, triggering events. This can become very difficult to maintain if not organized properly. Selenium suggest to use Page Object Model to define and organize the page related codes.

For example, we can define a LoginPage, with a login sumit form defined(a username element, a password element, and a submit button.)

## Share Sections among Pages

## Collection - Test Grouping



The structure of the test project looks like this:
![image](/assets/images/test-structure.png)