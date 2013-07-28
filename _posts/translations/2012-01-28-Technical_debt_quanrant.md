---
title: Technical Debt Quadrant 技术债象限
category: translation
tags: [technical debt]
layout: post
---

By _Martin Fowler_

There's been a few posts over the last couple of months about TechnicalDebt that's raised the question of what kinds of design flaws should or shouldn't be classified as Technical Debt.

前几个月的一些关于技术债的帖子引出了这样一个问题，什么样的设计缺陷应该或者不应该归为技术债。

<br/>

A good example of this is Uncle Bob's post saying a mess is not a debt. His argument is that messy code, produced by people who are ignorant of good design practices, shouldn't be a debt. Technical Debt should be reserved for cases when people have made a considered decision to adopt a design strategy that isn't sustainable in the longer term, but yields a short term benefit, such as making a release. The point is that the debt yields value sooner, but needs to be paid off as soon as possible.

一个很好的例子是Bob大叔的这篇“a mess is not a debt”的帖子。他的论调是那些对良好设计实践无知的人编写的腐坏的代码不应该是技术债。技术债应当是为了用于这样的情况的：人们为了一个获得一个短期的收益，如一次发布，经过慎重考虑而采取的不适用于长期存在的一个设计策略。重点是债务快速产生了价值，但是需要尽快偿还掉。

<br/>

To my mind, the question of whether a design flaw is or isn't debt is the wrong question. Technical Debt is a metaphor, so the real question is whether or not the debt metaphor is helpful about thinking about how to deal with design problems, and how to communicate that thinking. A particular benefit of the debt metaphor is that it's very handy for communicating to non-technical people.

在我看来，一个设计缺陷是不是一个技术债是一个错误的议题。技术债是一个隐喻，因此真正的议题是技术债隐喻是否对思考如何处理设计缺陷和如何交流思想有帮助。技术债隐喻的一个特有的好处是非常方便与非技术人员沟通。

<br/>

I think that the debt metaphor works well in both cases - the difference is in nature of the debt. A mess is a reckless debt which results in crippling interest payments or a long period of paying down the principal. We have a few projects where we've taken over a code base with a high debt and found the metaphor very useful in discussing with client management how to deal with it.

我认为技术债隐喻在两种情况下很好用-----不同点在于债务的本质。腐坏的代码是一种轻率的债务，会导致偿还非常沉重的利息或者花很长的时间来偿还本金。我们有那么几个项目代码库中存在非常高的债务，我们发现在与客户管理人员讨论如何去处理时，技术债这个隐喻非常有用。

<br/>

The debt metaphor reminds us about the choices we can make with design flaws. The prudent debt to reach a release may not be worth paying down if the interest payments are sufficiently small - such as if it were in a rarely touched part of the code-base.

技术债隐喻帮我们想起处理设计缺陷的方案。为了一个发布而谨慎引入的技术债如果需支付的利息足够的小可能就不需要去偿还它-例如如果是位于代码库中极少碰到的一块。

<br/>

So the useful distinction isn't between debt or non-debt, but between prudent and reckless debt.

因此有用的不是区分债不债的，而是区分草率的和谨慎的技术债。

<br/>

There's another interesting distinction in the example I just outlined. Not just is there a difference between prudent and reckless debt, there's also a difference between deliberate and inadvertent debt. The prudent debt example is deliberate because the team knows they are taking on a debt, and thus puts some thought as to whether the payoff for an earlier release is greater than the costs of paying it off. A team ignorant of design practices is taking on its reckless debt without even realizing how much hock it's getting into.

在我刚概述的例子中还有一个有意思的区分。不仅仅是要区分草率的和谨慎的技术债，而且要区分有意的和无意的技术债。谨慎的技术债是有意为之的，因为团队知道他们正在引入一个技术债，因此会去想是否一个更早的发布的收益会大于偿还它带来的成本。一个无视设计实践的团队会引入草率的技术债，甚至意识不到将会押上什么样的家当。

<br/>

Reckless debt may not be inadvertent. A team may know about good design practices, even be capable of practicing them, but decide to go "quick and dirty" because they think they can't afford the time required to write clean code. I agree with Uncle Bob that this is usually a reckless debt, because people underestimate where the DesignPayoffLine is. The whole point of good design and clean code is to make you go faster - if it didn't people like Uncle Bob, Kent Beck, and Ward Cunningham wouldn't be spending time talking about it.

草率的技术债可能不是无意造成的。一个团队也许知道良好的设计实践，甚至有能力去实践它们，但是却决定“快而烂”的干，因为她们支付不起编写整洁的代码所需要花费的时间。草率的技术债通常是因为人们对设计偿还期限的估计不足，在这点上我同意Bob大叔。良好设计和整洁代码的全部要点是让你走的更快。如果不是这样，像Bob大叔，Kent Beck, Ward Cunningham这些人就不会花时间去讨论它了。

<br/>

Dividing debt into reckless/prudent and deliberate/inadvertent implies a quadrant, and I've only discussed three cells. So is there such a thing as prudent-inadvertent debt? Although such a thing sounds odd, I believe that it is - and it's not just common but inevitable for teams that are excellent designers.

把技术债分为草率的/谨慎的和有意的/无意的隐含了象限的概念，我前面仅讨论了3个。那么有没有谨慎-无意的技术债呢？虽然听上去有点怪，我相信是有的-不那么常见但对于卓越设计者的团队是不可避免的。

<br/>

I was chatting with a colleague recently about a project he'd just rolled off from. The project that delivered valuable software, the client was happy, and the code was clean. But he wasn't happy with the code. He felt the team had done a good job, but now they realize what the design ought to have been.

我最近和一个同时聊起他刚离开的一个项目。项目交付了有价值的软件，客户高兴，代码整洁。但他对代码不满意。他感觉团队干得不错，但是他们刚意识到应该怎样去设计它。

<br/>

I hear this all the time from the best developers. The point is that while you're programming, you are learning. It's often the case that it can take a year of programming on a project before you understand what the best design approach should have been. Perhaps one should plan projects to spend a year building a system that you throw away and rebuild, as Fred Brooks suggested, but that's a tricky plan to sell. Instead what you find is that the moment you realize what the design should have been, you also realize that you have an inadvertent debt. This is the kind of debt that Ward talked about in his video.

我常常从最优秀的开发人员那听到这个。重点是当你在编程序，你在学习。情况往往是在你理解最好的设计方案应该是什么时，你已经在项目上编了1年的代码。也许人们应该这样规划项目，花一年的时间搭建一个系统后扔掉燃火重新搭建，就像Fred Brooks建议的那样，但是这个棘手的方案卖不出去。在你发现在你意识到设计应该是怎样时，你也意识到了你有了一个无意的技术债。这就是Ward在他的视频中提到的。

<br/>

The decision of paying the interest versus paying down the principal still applies, so the metaphor is still helpful for this case. However a problem with using the debt metaphor for this is that I can't conceive of a parallel with taking on a prudent-inadvertent financial debt. As a result I would think it would be difficult to explain to managers why this debt appeared. My view is this kind of debt is inevitable and thus should be expected. Even the best teams will have debt to deal with as a project goes on - even more reason not to recklessly overload it with crummy code.

在决定是支付利息还是支付本金时，隐喻对这种情况仍是非常有帮助的。然而技术债隐喻用于这种情况的一个问题是，我不能构思一个谨慎的-无意的金融债务的类似的情况。结果是，我认为很难向管理者解释为什么会产生这种债务。我的观点是这种债务是不可避免的，因此应该能料到。即使最好的团队随着项目进行也会有债务需要处理-即使足够理智地不去草率的堆叠糟糕的代码。

￼
![image](http://martinfowler.com/bliki/images/techDebtQuadrant.png)