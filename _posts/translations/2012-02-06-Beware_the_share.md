---
title: Beware the Share 当心公共代码
layout: post
category: translation
tags: [coding]
---

By _Udi Dahan_


iT WAS MY FiRST PROjECT AT THE COMPANY. I’d just finished my degree and was anxious to prove myself, staying late every day going through the existing code. As I worked through my first feature, I took extra care to put in place everything I had learned—commenting, logging, pulling out shared code into libraries where possible, the works. The code review that I had felt so ready for came as a rude awakening—reuse was frowned upon!

这是我来公司的第一个项目。我刚完成了我的学位，很想证明自己，每天都看代码待到很晚。我完成了我的第一个功能，我格外小心的用上了我学到的所有东西-注释，日志，把公共代码尽可能抽到库里。我觉得准备十足的代码复查却让我突然惊醒--重用被滥用了。

How could this be? Throughout college, reuse was held up as the epitome of quality software engineering. All the articles I had read, the textbooks, the seasoned software professionals who taught me—was it all wrong?

怎么可能？在大学期间，重用被视为软件工程质量的象征。我读过的所有文章，教科书，教过我的经验丰富的软件教授，他们都错了吗？

It turns out that I was missing something critical.

原来我漏掉了一些重要的东西。

Context.

上下文。

The fact that two wildly different parts of the system performed some logic in the same way meant less than I thought. Up until I had pulled out those libraries of shared code, these parts were not dependent on each other. Each could evolve independently. Each could change its logic to suit the needs of the system’s changing business environment. Those four lines of similar code were accidental—a temporal anomaly, a coincidence. That is, until I came along.

系统两个截然不同的部分以相同的方式执行了一段逻辑，这个事实并没我想的复杂。直到我把公共代码抽成库，这些部分并不彼此依赖。每一个都可以独立地改进。每一个都能修改它的逻辑以满足系统变化的商业环境。这几行相似的代码是偶然发生的--巧合而已。直到我的出现都是。

The libraries of shared code I created tied the shoelaces of each foot to the other. Steps by one business domain could not be made without first synchronizing with the other. Maintenance costs in those independent functions used to be negligible, but the common library required an order of magnitude more testing.

我创建的公共代码库把两只脚的鞋带绑在了一起。向业务领域的迈进必须要首先与另外一只进行同步。维护这些独立功能的成本原是可忽略不计的，但是公共库却需要大量的测试。

While I’d decreased the absolute number of lines of code in the system, I had increased the number of dependencies. The context of these dependencies is critical—had they been localized, the sharing may have been justified and had some positive value. When these dependencies aren’t held in check, their tendrils entangle the larger concerns of the system, even though the code itself looks just fine.

在我减少了系统中代码的绝对行数的同时，我增加了依赖的数量。这些依赖的上下文是重要的-如果在限定内使用，这些公用是合理的而且有积极的价值。当这些依赖不加制约，它们的触须会把系统更大的关注点缠绕在一起，尽管代码本身看上去很好。

These mistakes are insidious in that, at their core, they sound like a good idea. When applied in the right context, these techniques are valuable. In the wrong context, they increase cost rather than value. When coming into an existing codebase with no knowledge of where the various parts will be used, I’m much more careful these days about what is shared.

这些错误是潜在的，本质上它们听起来是个好主意。当应用在正确的上下文中时，这些技术是有价值的。在错误的上下文中，它们增加了成本而非价值。当进入一个现有的代码库而不知道各个部分被用在哪里时，我在什么是共用的上会非常小心。

Beware the share. Check your context. Only then, proceed.

谨慎使用共用。检查你的上下文。然后再行动。