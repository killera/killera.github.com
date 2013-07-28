---
title: Automate Your Coding Standard 自动化你的编码标准
layout: post
category: translation
tags: [coding]
---



By _Filip van Laenen_

YOU’VE PROBABLY BEEN THERE, TOO. At the beginning of a project, every- body has lots of good intentions—call them “new project’s resolutions.” Quite often, many of these resolutions are written down in documents. The ones about code end up in the project’s coding standard. During the kick-off meeting, the lead developer goes through the document and, in the best case, everybody agrees that they will try to follow them. Once the project gets underway, though, these good intentions are abandoned, one at a time. When the project is finally delivered, the code looks like a mess, and nobody seems to know how it came to be that way.

你一定也遇到过这种情况。在项目开始的时候，每个人都有许多好的想法---称为“新项目决议书”。往往，很多决议记录在文档中。关于代码的就是编码规范。在启动会议上，开发经理过一遍文档，最好的情况下，每个人都同意并遵守。然而，一旦项目启动，这些好的想法就一个一个被废弃了。当项目最终交付时，代码看上去一团糟，没有人知道为什么会变成那个样子。

When did things go wrong? Probably already at the kick-off meeting. Some of the project members didn’t pay attention. Others didn’t understand the point. Worse, some disagreed and were already planning their coding standard rebellion. Finally, some got the point and agreed, but when the pressure in the project got too high, they had to let something go. Well-formatted code doesn’t earn you points with a customer that wants more functionality. Furthermore, following a coding standard can be quite a boring task if it isn’t automated. Just try to indent a messy class by hand to find out for yourself.

事情在什么时候出的错？大概在启动会议时就已经错了。一些项目组成员没当回事。另一些不明白为什么这么干。更有甚者，一些人不同意并且已经在计划他们自己的编码规范。最终，一些人明白并且赞同，但当项目压力太大时，他们不得不舍掉一些东西。组织良好的代码并不能给你在一个想要更多功能的客户那为你加分。进一步讲，如果没有自动化，遵循一种编码规范是一件相当烦的事。试试手工去调整一个乱糟糟的类的缩进你就知道了。

But if it’s such a problem, why is it that we want a coding standard in the first place? One reason to format the code in a uniform way is so that nobody can “own” a piece of code just by formatting it in his or her private way. We may want to prevent developers from using certain antipatterns in order to avoid some common bugs. In all, a coding standard should make it easier to work in the project, and maintain development speed from the beginning to the end. It follows, then, that everybody should agree on the coding standard, too—it does not help if one developer uses three spaces to indent code, and another uses four.

但如果这是个问题，为什么我们在开始想要一个编码规范呢？以统一方式规范代码的一个原因是没有人能以他自己私有的方式来“占有”一段代码。我们可能想阻止程序员用一些反模式来避免一些常规的错误。总的说来，一个编码规范应该能很容易在项目中应用起来，自始至终保持开发速度。然后，每个人也都应当遵守编码规范--不要一个程序员用3个空格缩进代码，其他人用4个。

There exists a wealth of tools that can be used to produce code quality reports and to document and maintain the coding standard, but that isn’t the whole solution. It should be automated and enforced where possible. Here are a few examples:

有很多工具可以生成代码质量报告并记录维护编码规范，但并不能解决所有问题。编码规范应该是尽可能规范化并强制执行的。这有一些例子：

Make sure code formatting is part of the build process, so that everybody runs it automatically every time they compile the code.

确保代码格式成为构建过程的一部分，这样每个人在每次编译代码时都会自动运行它。

Use static code analysis tools to scan the code for unwanted antipatterns. If any are found, break the build.

用静态的代码分析工具扫描代码中的不必要的反模式。如果有，让构建挂掉。

Learn to configure those tools so that you can scan for your own, project- specific antipatterns.

学会配置这些工具以便你可以自己扫描特定项目的反模式。

Do not only measure test coverage, but automatically check the results, too. Again, break the build if test coverage is too low.

不要只衡量测试覆盖率，也要自动检查结果。同样，如果测试覆盖率太低就让构建挂掉。

Try to do this for everything that you consider important. You won’t be able to automate everything you really care about. As for the things that you can’t automatically flag or fix, consider them a set of guidelines supplementary to the coding standard that is automated, but accept that you and your colleagues may not follow them as diligently.

试着对你认为重要的每件事这样做。你不可能自动化你真正关心的所有事。对于那些你不能自动标示或修复的事，考虑把它们作为自动化编码规范的补充指导，允许你和你的同事不统统遵循它。

 Finally, the coding standard should be dynamic rather than static. As the proj- ect evolves, the needs of the project change, and what may have seemed smart in the beginning isn’t necessarily smart a few months later.

最后，编码规范应当动态而非静态。随着项目进行，项目要求也发生变化，一些在开始看上去很漂亮讲究的规范在几个月后就没那么有必要了。
