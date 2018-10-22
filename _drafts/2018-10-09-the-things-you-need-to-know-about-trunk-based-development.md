---
title: The things you need to know about Truck-Based-Development
category: agile  
tags: [agile, truck based development, feature toggle, feature branches, git flow]  
layout: post  
lang: en
---

## Definition



**Trunk Based Development(TBD)** is a branching model, the whole dev team will develop and collaborate on a single branch traditionally called **Trunk**. More and more agile teams adopts this popular practice nowadays.

![image](/assets/images/trunk_based_development.png)

(talk about there is not an rules applys to all cases, embracing changes, teams should make changes according to their team's situation.)

Next, we will explain more details about **TBD**.

(Note: there are many different version control tools, such as git, tortise hg, TFS, svn, ...., the usage and ideas of them might be dramatically different, I have used some different tools in different projects, I have to say Git is a very friendly and excellent tools for modern development teams. So what we discuss below is mostly based on Git, but the idea is also suitable to some other version control tools.)

## Explanation

It's not possible for a team to adopt TBD just from its definition. In the next, we will mention the several important aspects need to consider when doing TBD:

### TBD needs supports from other agile practices

**Work on trunk only** is not a real **Trunk Based Development**. There is a reason that why TBD is used widely in agile teams. Some good agile practices, such as Test Driven Development(TDD), Continuous Integration(CI), Continuous Delivery(CD), are the fundamental of TBD.

**TDD** has two characters: short and correctness. TDD development cycle is very short, which make frequent commit possible. And on the other hand, the tests make sure that the code meet the requirements. The test coverage is also guarentted with TDD. With the help of TDD, team members are able to commit to the trunk branch frequently and confidently.

<a title="By Xarawn [CC BY-SA 4.0 
 (https://creativecommons.org/licenses/by-sa/4.0
)], from Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:TDD_Global_Lifecycle.png"><img width="1024" alt="TDD Global Lifecycle" src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/TDD_Global_Lifecycle.png/1024px-TDD_Global_Lifecycle.png"></a>


Another benefit is that because the team always work on the latest codebase, the possibility of huge merge conflicts is reduced.

**Continuous Integration (CI)** is a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build, allowing teams to detect problems early[^1]. With CI environment set up, the team are able to see the integration progress and result of each commit. If the build and test succeed, it means that the "integration" succeed. CI gives the team fast feedback, and in the meantime, TBD makes the integration more frequently. Team members can use others' code snippet without waiting for a long time.

Just as Martin Fowler defined in his blog[^2]:

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.

He highlighted:

> The key test is that a business sponsor could request that **the current development version of the software can be deployed into production at a moment's notice**.

TBD is exactly an branching model that make continuous delivery possible. TBD requires the release is created from trunk branch, which mimics the risk of having 


### TBD needs an agreement and discipline among the whole team

### TBD is not **Trunk-only**, it works well with **short-lived feature branches**

### Consider your team's structure  

PR/code review

### Make trunk branch in a releasable state.


It works well with **short-lived feature branches**, some people like to push this model to extreme --- commiting staight to the trunk. 

I introduced **Trunk Based Development(TBD)** to some teams which rely on feature branches heavily, but for another team which is working on a single **trunk**  development branch, I tried to persuade them to start using feature branches. It seems that I am trying to introduce totally different practices to different teams, in the next I will talk about my reasons and the diffrences between them.

## Branching models

Here we will talk about two widely used branching models: **Trunk Based Development** and **Git-flow**.

### Trunk Based Development


From the diagram, we can see:
1. Team members work on trunk branch;
2. Short-lived feature branch is allowed;
3. Don't commit on release branch directly to avoid changes not merged back to trunk if committed on release branch. the changes will be done on trunk branch, and then cherry-pick to relase branch as hotfix.

### Git-flow

**Git-flow** is another branching model which is usually compared with Trunk Based Development. It has serveral branches, such as develop, release, hotfix, feature branches, master, and it has strict rules on branching out and merging in. Below is the git-flow branching chart.

![image](/assets/images/GitFlowHotfixBranch.png)

From the diagram we can see:

1. Master branch is always release ready.
2. Changes can be made directly on non-develop branch(release branches, hotfixes, feature branches), and then merge back to develop at some point.
3. Hotfixes are made on their own branches and then merge back to master and develop.
4. Feature branches is likely to be **long-lived**.

## Comparison

In most traditional development teams, people likely follow branching models like Git-flow. One possible reason is that the version control tool doesn't not support a friendly and easy way to support frequent branching and merging.
The other one is that people don't want to commit incompleted features to code base, or people don't want others' breaking changes affect themselves.

TBD is adpoted by more and more agile teams, as the team member can always work on the latest code. Because team members push changes to trunk frequently, there will be no big merge issues. Another benefit is, the CI/CD automation is more fluent, as the team don't need to care about which branch will deployed to which environment.

The truth is that you can't expect that a new team member can follow your team rules or principles without understanding the real meaning of it.

Right time, Right place, Right people. It depends. Not all the practices suits all the teams. Every team is different. you can't just always apply one set of practices to different teams.

## Suggestion

When to use branches:
1. when need PR
2. when there are junior developers.
3. your team member might hold all the changes in his/her local machine for days before he finish the story.
4. Try something
5. On projects that don't have enough tests.

Notes:
1. short-lived feature branches.
2. merge back to trunk if possible.
3. merge trunk to feature branches as frequent as possible.
4. Do not hold the codes in your local machine.
4. Configure CI to run bulid/test for feature branches.



[^1]: [https://www.thoughtworks.com/continuous-integration](https://www.thoughtworks.com/continuous-integration)
[^2]: [https://martinfowler.com/bliki/ContinuousDelivery.html](https://martinfowler.com/bliki/ContinuousDelivery.html)

## References

[https://trunkbaseddevelopment.com/](https://trunkbaseddevelopment.com/)

[https://www.thoughtworks.com/continuous-integration](https://www.thoughtworks.com/continuous-integration)

[https://martinfowler.com/bliki/ContinuousDelivery.html](https://martinfowler.com/bliki/ContinuousDelivery.html)

https://nvie.com/posts/a-successful-git-branching-model/

http://kean.github.io/post/trunk-based-development
https://trunkbaseddevelopment.com/alternative-branching-models/

https://www.thoughtworks.com/insights/blog/enabling-trunk-based-development-deployment-pipelines

