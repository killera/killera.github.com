---
title: Before You Refactor 重构之前
layout: post
category: translation
tags: [refactor]
---

By _Rajith Attapattu_

AT SOME POiNT,  every programmer will need to refactor existing code. But before you do so, please think about the following, as this could save you and others a great deal of time (and pain):

在有些时候，每个程序员都需要重构现有的代码。但在你做之前，请想一下下面的事项，这能节省你和其他人的很多时间(和痛苦)。

The best approach for restructuring starts by taking stock of the existing codebase and the tests written against that code. This will help you under- stand the strengths and weaknesses of the code as it currently stands, so you can ensure that you retain the strong points while avoiding the mistakes. We all think we can do better than the existing system...until we end up with something no better—or even worse—than the previous incarnation because we failed to learn from the existing system’s mistakes.

重构最好的办法是先对现有代码库和相关测试代码归纳总结一下。这会帮你了解现在代码中的优点和缺点，这样你就可以保证能保持住优点而避免犯错误。我们都认为我们能比现有系统做的更好....直到我们以一些比原来不是那么好-或是更糟的代码而结束，因为我们没能从现有系统的错误中吸取教训。

Avoid the temptation to rewrite everything. It is best to reuse as much code as possible. No matter how ugly the code is, it has already been tested, reviewed, etc. Throwing away the old code—especially if it was in production—means that you are throwing away months (or years) of tested, battle-hardened code that may have had certain workarounds and bug fixes you aren’t aware of. If you don’t take this into account, the new code you write may end up showing the same mysterious bugs that were fixed in the old code. This will waste a lot of time, effort, and knowledge gained over the years.

抵挡住重写一切的诱惑。最好重用尽可能多的代码。不管代码多么丑陋，它都是已经测试的，检查过的。扔掉旧代码---尤其是产品代码，意味着你正在扔掉经过数月（或者数年）测试过的，久经沙场的代码，在其中有一些你不知道的临时解决方案和bug修复。如果你没有考虑这些，你写的新代码将会重现这些在旧代码中已经修复过的诡异的bug。这将浪费很多时间，努力及多年获得的知识。

Many incremental changes are better than one massive change. Incremental changes allows you to gauge the impact on the system more easily through feedback, such as from tests. It is no fun to see a hundred test failures after you make a change. This can lead to frustration and pressure that can in turn result in bad decisions. A couple of test failures at a time is easier to deal with, leading to a more manageable approach.

多次增量修改胜过一次重大的调整。增量修改能让你更容易的通过反馈，比如测试来判断对系统造成的影响。在你修改完之后看到百十个测试失败不是一件好事。这会导致挫败感和压力，从而做出错误的决定。一次一两个测试失败很容易对付，能很容易的处理。

After each development iteration, it is important to ensure that the existing tests pass. Add new tests if the existing tests are not sufficient to cover the changes you made. Do not throw away the tests from the old code without due consideration. On the surface, some of these tests may not appear to be applicable to your new design, but it would be well worth the effort to dig deep down into the reasons why this particular test was added.

在每个开发迭代之后，确保现有测试通过是很重要的。如果现有测试不足以覆盖你的修改就添加新的测试。不要在欠考虑的情况下扔掉原有代码的测试。表面上，一些测试可能看上去不适用于你的新设计，但是它值得你好好的去挖掘一下添加这个特定测试的原因。

Personal preferences and ego shouldn’t get in the way. If something isn’t broken, why fix it? That the style or the structure of the code does not meet your personal preference is not a valid reason for restructuring. Thinking you could do a better job than the previous programmer is not a valid reason, either.

抛掉自我和个人喜好。如果没有问题，为什么去修它呢？代码的结构或者风格不符合你的个人喜好不是一个正当的重构理由。同样，认为你能比原先的程序员干的更好也不是一个正当的理由。

New technology is an insufficient reason to refactor. One of the worst reasons to refactor is because the current code is way behind all the cool technology we have today, and we believe that a new language or framework can do things a lot more elegantly. Unless a cost-benefit analysis shows that a new language or framework will result in significant improvements in functionality, maintainability, or productivity, it is best to leave it as it is.

新技术不是重构的充分理由。最差的重构理由之一是现有代码对于我们今天所有酷的技术太古老了，我们相信新技术或者框架能更优雅的完成工作。除非有成本效益分析表明新技术或者框架能在功能性、可维护性或者生产力上有显著提升，否则最好让代码保持现状。

Remember that humans make mistakes. Restructuring will not always guarantee that the new code will be better—or even as good as—the previous attempt. I have seen and been a part of several failed restructuring attempts. It wasn’t pretty, but it was human.

记住人总会犯错。重构不能总保证新代码更好--或者和原代码一样好。我曾看到也参与过几个失败的重构尝试。人非圣贤，孰能无过。
