---
title: You may not know Truck-Based-Development
category: agile  
tags: [agile, truck based development, feature toggle, feature branches, git flow]  
layout: post  
lang: en
---

I introduced **Trunk Based Development(TBD)** to some teams which rely on feature branches heavily, but for another team which is working on a single **trunk**  development branch, I tried to persuade them to start using feature branches. It seems that I am trying to introduce totally different practices to different teams, in the next I will talk about my reasons and the diffrences between them.

# Branching models

Here we will talk about two widely used branching models: **Trunk Based Development** and **Git-flow**.

## Trunk Based Development

**Trunk Based Development(TBD)** is a branching model, the whole dev team will develop and collaborate on a single branch traditionally called **Trunk**. More and more agile teams adopts this popular practice nowadays. It works well with **short-lived feature branches**, some people like to push this model to extreme --- commiting staight to the trunk.  

![image](/assets/images/trunk_based_development.png)

From the diagram, we can see:
1. Team members work on trunk branch;
2. Short-lived feature branch is allowed;
3. Don't commit on release branch directly to avoid changes not merged back to trunk if committed on release branch. the changes will be done on trunk branch, and then cherry-pick to relase branch as hotfix.

## Git-flow

**Git-flow** is another branching model which is usually compared with Trunk Based Development. It has serveral branches, such as develop, release, hotfix, feature branches, master, and it has strict rules on branching out and merging in. Below is the git-flow branching chart.

![image](/assets/images/GitFlowHotfixBranch.png)

From the diagram we can see:

1. Master branch is always release ready.
2. Changes can be made directly on non-develop branch(release branches, hotfixes, feature branches), and then merge back to develop at some point.
3. Hotfixes are made on their own branches and then merge back to master and develop.
4. Feature branches is likely to be **long-lived**.

# Comparison

In most traditional development teams, people likely follow branching models like Git-flow. One possible reason is that the version control tool doesn't not support a friendly and easy way to support frequent branching and merging.
The other one is that people don't want to commit incompleted features to code base, or people don't want others' breaking changes affect themselves.

TBD is adpoted by more and more agile teams, as the team member can always work on the latest code. Because team members push changes to trunk frequently, there will be no big merge issues. Another benefit is, the CI/CD automation is more fluent, as the team don't need to care about which branch will deployed to which environment.

The truth is that you can't expect that a new team member can follow your team rules or principles without understanding the real meaning of it.

Right time, Right place, Right people. It depends. Not all the practices suits all the teams. Every team is different. you can't just always apply one set of practices to different teams.

# Suggestion

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



[https://trunkbaseddevelopment.com/](https://trunkbaseddevelopment.com/)

https://nvie.com/posts/a-successful-git-branching-model/

http://kean.github.io/post/trunk-based-development
https://trunkbaseddevelopment.com/alternative-branching-models/

https://www.thoughtworks.com/insights/blog/enabling-trunk-based-development-deployment-pipelines

