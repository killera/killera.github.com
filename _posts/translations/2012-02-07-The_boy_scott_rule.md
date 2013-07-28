---
title: The Boy Scout Rule 童子军规则
category: translation
tags: [coding]
layout: post
---

By _Robert C. Martin (Uncle Bob)_

THE BOY SCOUTS HAVE A RULE: “Always leave the campground cleaner than you found it.” If you find a mess on the ground, you clean it up regardless of who might have made it. You intentionally improve the environment for the next group of campers. (Actually, the original form of that rule, written by Robert Stephenson Smyth Baden-Powell, the father of scouting, was “Try and leave this world a little better than you found it.”)

童子军有个规矩：“始终保持露营地比你发现它时干净”。如果你在地上发现了一块脏东西，不管是谁弄的，都清理掉它。要为了下一拨露营者刻意的改善环境。（实际上，那条规矩的早期版本，出自Robert Stephenson Smyth Bden-Powell，童子军活动之父，说的是“努力使世界比你发现它时变得更好”。）

What if we followed a similar rule in our code: “Always check a module in cleaner than when you checked it out”? Regardless of who the original author was, what if we always made some effort, no matter how small, to improve the module? What would be the result?

假使我们在我们代码中遵守相似的规则呢, “ 永远保持你check in的模块比你check out时干净”。不管谁是原作者，如果我们一直做一些努力，无论多小，来改善模块，结果会怎样？

I think if we all followed that simple rule, we would see the end of the relentless deterioration of our software systems. Instead, our systems would gradually get better and better as they evolved. We would also see teams caring for the system as a whole, rather than just individuals caring for their own small part.

我认为如果我们遵守这个简单的规则，我们会看到我们的软件系统不再腐坏恶化。相反，我们的系统会因为改进而变得越来越好。我们也会看到团队将系统当做一个整体来看待，而不是只单独的关系他们自己的那一小部分。

I don’t think this rule is too much to ask. You don’t have to make every module perfect before you check it in. You simply have to make it a little bit better than when you checked it out. Of course, this means that any code you add to a module must be clean. It also means that you clean up at least one other thing before you check the module back in. You might simply improve the name of one variable, or split one long function into two smaller functions. You might break a circular dependency, or add an interface to decouple policy from detail.

我不认为这条规矩要求过分。你不必在你check in时把每个模块搞完美。你只要简单的让它比你check out时好一点。当然，这意味着你你添加到模块的任何代码都是整洁的。也意味着在你check in 模块时至少清理了其他的一处代码。你可以简单地改进一个变量的名字，或者把一个长函数拆成两个较小的函数。你可以解除一个环形依赖，或者添加接口以解耦实现细节与对外提供的功能。

Frankly, this just sounds like common decency to me—like washing your hands after you use the restroom, or putting your trash in the bin instead of dropping it on the floor. Indeed, the act of leaving a mess in the code should be as socially unacceptable as littering. It should be something that just isn’t done.

坦白讲，这对我来说听上去就像普通的礼仪-就像上完厕所要洗手，把垃圾放到垃圾桶而不是扔在地板上。确实，在代码中遗留垃圾就像社会上扔垃圾一样不可接受。这是不合乎礼仪的。

But it’s more than that. Caring for our own code is one thing. Caring for the team’s code is quite another. Teams help one another and clean up after one another. They follow the Boy Scout rule because it’s good for everyone, not just good for themselves.

但还远不止这些，关心我们自己的代码是一件事。关心团队的代码完全是另外一码事。团队互相帮助，互相清理代码。他们遵守童子军规则，因为这对每个人都好，不仅仅对他们自己。