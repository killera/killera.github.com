---
title: Choose Your Tools with Care 谨慎选择你的工具
category: translation
tags: [open source]
layout: post
---

By _Giovanni Asproni_

MODERN APPLiCATiONS ARE VERY RARELY BUiLT FROM SCRATCH. They are assembled using existing tools—components, libraries, and frameworks— for a number of good reasons:

现代的应用很少从零开始构建。它们由现有的工具-组件，库和框架组装，下面是一些很好的理由：

<br/>

Applications grow in size, complexity, and sophistication, while the time available to develop them grows shorter. It makes better use of developers’ time and intelligence if they can concentrate on writing more business-domain code and less infrastructure code.

当可用的开发时间越来少，应用变得庞大，复杂并且混杂。如果开发人员能把时间用于更多的编写业务领域代码而非基础设施代码，可以更好的使用他们的时间和智慧。

<br/>

Widely used components and frameworks are likely to have fewer bugs than the ones developed in-house.

广泛使用的组件和框架很可能比自己内部开发的代码有更少的bug。

<br/>

There is a lot of high-quality software available on the Web for free, which means lower development costs and greater likelihood of finding developers with the necessary interest and expertise.

在网上有很多免费的高质量软件，这意味着更少的开发成本和找到有必要兴趣和经验的开发人员。

<br/>

Software production and maintenance is human-intensive work, so buying may be cheaper than building.

软件产品开发和维护是劳动密集型工作，因此购买比构建更便宜。

<br/>

However, choosing the right mix of tools for your application can be a tricky business requiring some thought. In fact, when making a choice, you should keep in mind a few things:

然而，为你的应用选择正确的工具集是需要费脑子的。事实上，在选择时，你应该记住以下几件事：

<br/>

Different tools may rely on different assumptions about their context—e.g., surrounding infrastructure, control model, data model, communication protocols, etc.—which can lead to an architectural mismatch between the application and the tools. Such a mismatch leads to hacks and workarounds that will make the code more complex than necessary.

不同的工具依赖于不同的环境-比如，周边基础架构，控制模型，数据模型，通讯协议，等等，这可能导致应用和工具之间架构的不匹配。这样的不匹配导致的硬编码和临时解决方案会导致代码不必要的复杂。

<br/>

Different tools have different life cycles, and upgrading one of them may become an extremely difficult and time-consuming task since the new functionality, design changes, or even bug fixes may cause incompatibilities with the other tools. The greater the number of tools, the worse the problem can become.

不同的工具有不同的生命周期，升级会变成一个十分困难和耗时的工作，因为新的功能，设计改变，或者甚至bug修复都会造成与其他工具的不兼容。工具数量越多，问题越严重。

<br/>

Some tools require quite a bit of configuration, often by means of one or more XML files, which can grow out of control very quickly. The application may end up looking as if it was all written in XML plus a few odd lines of code in some programming language. The configurational complexity will make the application difficult to maintain and to extend.

一些工具需要大量的配置，通常借助于一个或多个xml文件，会很快变得不可控。应用最后看起来像是用xml编写加上一些编程语言的代码。配置的复杂性使得应用难于维护和扩展

<br/>

Vendor lock-in occurs when code that depends heavily on specific ven- dor products ends up being constrained by them on several counts: maintainability, performances, ability to evolve, price, etc.

当代码严重依赖于特定厂商的产品时会发生厂商锁定，通常在一下几个方面：可维护性，性能，改进的能力，价格等等。

<br/>

If you plan to use free software, you may discover that it’s not so free after all. You may need to buy commercial support, which is not necessarily going to be cheap.

如果你打算使用免费软件，你会发现并不是完全免费。你需要购买商业支持，绝对的不便宜。

<br/>

Licensing terms matter, even for free software. For example, in some companies, it is not acceptable to use software licensed under the GNU license terms because of its viral nature—i.e., software developed with it must be distributed along with its source code.

许可证的问题，即使是对免费软件而言。例如在一些公司因为GNU许可的“病毒特性(viral nature)”不允许使用该许可的软件-比如，使用GNU软件开发的软件必须同源代码一起发布。

<br/>

My personal strategy to mitigate these problems is to start small by using only the tools that are absolutely necessary. Usually the initial focus is on removing the need to engage in low-level infrastructure programming (and problems), e.g., by using some middleware instead of using raw sockets for distributed applications. And then add more if needed. I also tend to isolate the external tools from my business domain objects by means of interfaces and layering, so that I can change the tool if I have to with a minimal amount of pain. A positive side effect of this approach is that I generally end up with a smaller application that uses fewer external tools than originally forecast.

我的减少这些问题的个人策略是只是用绝对必要的工具。通常最初的焦点是去掉对低水平的构建编程（和问题）的需求，例如，在分布式应用中使用一些中间件替代原始的socket。然后按需加入更多的工具。我还倾向于通过接口和分层机制隔离外部工具和我的业务领域对象，这样如果使用中有一点点痛苦，我可以改换工具。这种方法的好处是我通常能使用更少的外部工具完成一个较小的应用。
