---
title: Act with Prudence 三思而后行
layout: post
category: translation
tags: [technical debt]
---


By _Seb Rose_

>Whatever you undertake, act with prudence and consider the consequences.
>>—Anon
>无论做什么事情，都要三思而后行。-Anon

NO MATTER HOW COMFORTABLE A SCHEDULE LOOKS at the beginning of an iteration, you can’t avoid being under pressure some of the time. If you find yourself having to choose between “doing it right” and “doing it quick,” it is often appealing to “do it quick” with the understanding that you’ll come back and fix it later. When you make this promise to yourself, your team, and your customer, you mean it. But all too often, the next iteration brings new prob- lems and you become focused on them. This sort of deferred work is known as technical debt, and it is not your friend. Specifically, Martin Fowler calls this deliberate technical debt in his taxonomy of technical debt,* and it should not be confused with inadvertent technical debt.

在一个迭代之初，不论你计划的如何有条不紊，有时你仍然会感到压力重重。如果要你在“干好”和“快点交工”之间权衡，人们往往会倾向于“快点交工”，然后回头有时间把它修好。当你向自己，你的团队和你的客户许下承诺时，你也是这么想的。但往往是，下一个迭代你又忙于出现的一些新问题。这些推迟的工作被称为技术债，并不是什么好事。Martin Fowler在他的技术债分类学中将其称为deliberate technical debt（故意技术债）[^1]，以便和inadvertent technical debt（无意技术债）区分。


Technical debt is like a loan: you benefit from it in the short term, but you have to pay interest on it until it is fully paid off. Shortcuts in the code make it harder to add features or refactor your code. They are breeding grounds for defects and brittle test cases. The longer you leave it, the worse it gets. By the time you get around to undertaking the original fix, there may be a whole stack of not-quite-right design choices layered on top of the original problem, making the code much harder to refactor and correct. In fact, it is often only when things have got so bad that you must fix the original problem, that you actually do go back to fix it. And by then, it is often so hard to fix that you really can’t afford the time or the risk.

技术债就像债务：你短期会从中获益，但在偿清前你要付利息。满足一时之需的代码使你难于添加新功能或者进行重构。它们是滋生缺陷和脆弱的测试用例的土壤。留它越久，危害越大。当你抽得时间来解决最初的那个问题时，将会有一堆不那么正确的设计方案掩盖于原来的问题之上，使代码愈发难于重构和修正。事实上，也仅当事情坏到你必须要修掉原来那个问题时，你才会真去修它。到那时因为你负担不起高昂的时间或者风险 ，修复它往往非常困难。

There are times when you must incur technical debt to meet a deadline or implement a thin slice of a feature. Try not to be in this position, but if the situ- ation absolutely demands it, then go ahead. But (and this is a big but) you must track technical debt and pay it back quickly, or things go rapidly downhill. As soon as you make the decision to compromise, write a task card or log it in your issue-tracking system to ensure that it does not get forgotten.

有时候为了应付最后期限或者实现一个小功能，你不得不引入技术债。尽量不要这么干，除非事态紧急。如果这么干了，你必须跟踪技术债并尽快偿清，否则事态会急转直下。一旦你决定妥协时，写一张任务卡片或者把它记录在你的问题跟踪系统，确保不要忘掉。

If you schedule repayment of the debt in the next iteration, the cost will be minimal. Leaving the debt unpaid will accrue interest, and that interest should be tracked to make the cost visible. This will emphasize the effect on Busi-ness value of the project’s technical debt and enables appropriate prioritization of the repayment. The choice of how to calculate and track the interest will depend on the particular project, but track it you must.

如果你把债务偿还计划在下一个迭代，成本是最小的。不偿还债务会增加利息，必须跟踪利息使成本可见。这样会突出项目的技术债对商业价值的影响，促使安排合适的优先级来偿还债务。选择怎样计算并跟踪利息取决于特定的项目，但你必须去做。

Pay off technical debt as soon as possible. It would be imprudent to do otherwise.

尽快的偿清技术债，否则是不明智的。

[^1]: http://martinfowler.com/bliki/TechnicalDebtQuadrant.html