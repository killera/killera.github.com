---
title: "Regression Test 101 - 2 - Define Test Target"  
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


Today's topic is **Regression Test Target**.

According to Test Pyramid[^1], we should have fewer tests in high-level like UI. So it's not recommended to add too many UI tests.

<figure>
<img src="/assets/images/testpyramid.png">
<figcaption>https://martinfowler.com/articles/practical-test-pyramid.html
</figcaption>
</figure>

## Smoke Test

The first step might be to add some "Smoke Test". The smoke test is a set of minimun test cases which covers the basic functionality of the appliation. It will not go to deep into the application functionalites.

Smoke tests usually run fast(keep it less than 10 minutes), it can be triggered for each commit.


## Full Regression Test.

If the smoke tests are not enough, the next step might be adding more detailed regression tests. UI test usually run slow, it is not recommended to run for every commit. Other than triggerred every commit, it can be triggered periodically, such as daily, or every n hours.

The regression tests should be focusing on the functionality other than page layout, otherwise, the test can become very unstable and difficult to maintain.


[^1]: [https://martinfowler.com/articles/practical-test-pyramid.html#UiTests](https://martinfowler.com/articles/practical-test-pyramid.html#UiTests)