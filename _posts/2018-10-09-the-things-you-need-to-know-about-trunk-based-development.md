---
title: The things you need to know about Trunk-Based-Development
category: agile  
tags: [agile, trunk based development, feature toggle, feature branches, git flow]  
layout: post  
lang: en
---

**Trunk Based Development(TBD)** is a branching model, the whole dev team will develop and collaborate on a single branch traditionally called **Trunk**[^0]. More and more agile teams adopt this popular practice nowadays. Here is a diagram of TBD: 

![image](/assets/images/trunk_based_development.png)

TBD is very simple and straightforward from the diagram, but I found that different teams might have totally different understandings (or misunderstanding). Every team is different, people can make changes according to their own cases, but there are some key points which should be always keep in mind when doing TBD. This is also what I will talk about in the next section.

```console 
Note: there are many different version control tools, such as git, Mercurial, TFS, SVN, ClearCase, ... The usages and ideas behind might be dramatically different. What we discuss below is mostly based on Git, but the idea might be also suitable to other git-like tools.
```

### TBD needs supports from other agile practices

**Work on trunk only** is not a real **Trunk Based Development**. There is a reason that why TBD is used widely in agile teams. Some good agile practices, such as Test Driven Development(TDD), Continuous Integration(CI) are essential to TBD.

**TDD** has two characters: short and correctness. TDD development cycle is very short, which make frequent commit possible. And on the other hand, the tests make sure that the code meet the requirements. The test coverage is also guaranteed with TDD. With the help of TDD, team members are able to commit to the trunk branch frequently and confidently.

<a title="By Xarawn [CC BY-SA 4.0 
 (https://creativecommons.org/licenses/by-sa/4.0
)], from Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:TDD_Global_Lifecycle.png"><img width="1024" alt="TDD Global Lifecycle" src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/TDD_Global_Lifecycle.png/1024px-TDD_Global_Lifecycle.png"></a>


Another benefit is that because the team always work on the latest codebase, the possibility of huge merge conflicts is reduced.

**Continuous Integration (CI)**[^1] is a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build, allowing teams to detect problems early.

With CI environment set up, the team are able to see the integration progress and result of each commit. If the build and test succeed, it means that the "integration" succeed. 

CI gives the team fast feedback, and in the meantime, TBD makes the integration more frequently. Team members can use others' code snippet without waiting for a long time.

### Keep trunk branch release ready.

Just as Martin Fowler defined in his blog[^2]:

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.

He highlighted:

> The key test is that a business sponsor could request that **the current development version of the software can be deployed into production at a moment's notice**.

TBD is exactly the branching model that make continuous delivery possible. TBD requires the release package is created from trunk branch, which mimics the risk of having different releases from some not fully tested branches. On the other hand, the trunk based development is exactly the right branching model to support a frequent release pace.

### TBD requires team-level agreement and discipline

TBD is a practice, which means that all team members need to agree on the basic disciplines, such as:

* Keeping a good test coverage
* Fix the build quickly when it failed
* Commit/Merge into trunk branch as frequently as possible, 
* Make the trunk branch in a releasable state 
* Run unit test (at least) on local machine before commit. 
  
If there is a way to make the integration faster and more stable, adopt it; if there is something hurts

### TBD is not **Trunk-only**

Some people think "Trunk Based Development" as "Trunk-only Development", which is not correct. TBD works well with **short-lived feature branches**. In some cases, people still need to work on a "short-lived" feature branch, and then merge it back to trunk when possible. 

For example, one team member can work on a story which may take one or two days, it may have some changes to the database tables and data. It makes sense that he/she put the code in a separate branch and merge it back to trunk later. There are several additional benefit by doing this:

* By committing the branch to codebase, reduced the risk of having the code on someone's local machine.
* The build and test can run against the branch on CI.
* People can create PR(pull request), do code review, and then merge into trunk branch. This is especially important and useful when some members in your team don't have enough domain knowledge, or feature context, or even development skills.
* People can merge branch into trunk when he/she thinks it's OK, don't need to wait the whole story finished.
* People can merge latest trunk branch code in when needed. 

### Use "Brach by Abstraction" or "Feature Toggles"

Sometimes, people might work on big changes, for example, replacing framework or module, which might take a longer time. In this case, you may not need a separate branch. With **Branch by Abstraction**[^4], you can finish the big task gradually.

Another situation is that, you are working on a new feature, which may contain 5 stories. You may need to only show the feature to user when all the stories are finished. In this case, you can use **Feature Toggles**[^5] to switch off the feature before it's fully implemented.

### Create release branches only if necessary

There is not necessary to create release branch to do a release. A release tag on trunk branch is enough as a start. If you need to hotfix, then create branch.

Release branches will stop at some point, for example, the previous releases are ok to remove from codebase when new release is deployed.

Some people commit hotfixes directly on release branch, please remember to merge back to trunk as well. If possible, please always add hotfixes on trunk branch and then **cherry-pick**[^3] into release branch. By doing this, you won't forget to merge the code to trunk.


## Git-flow

**Git-flow** is another branching model which is usually compared with Trunk Based Development. It has several branches, such as develop, release, hotfix, feature branches, master, and it has strict rules on branching out and merging in. Below is the git-flow branching chart.

![image](/assets/images/GitFlowHotfixBranch.png)

From the diagram we can see:

1. Master branch is always release ready.
2. Changes can be made directly on non-develop branch(release branches, hotfixes, feature branches), and then merge back to develop at some point.
3. Hotfixes are made on their own branches and then merge back to master and develop.
4. Feature branches is likely to be **long-lived**.

Some team might be more familiar with Git-flow, or the CI is already configured according to Git-flow. But please remember to merge from/into develop branch often to avoid big conflict, and keep feature branches **short-lived**. 



[^0]: Some teams use `develop` branch as trunk branch, others use `master` branch

[^1]: See on ThoughtWorks' [website](https://www.thoughtworks.com/continuous-integration)

[^2]: See Martin Fowler's blog: [https://martinfowler.com/bliki/ContinuousDelivery.html](https://martinfowler.com/bliki/ContinuousDelivery.html)

[^3]: Only pick one/some existed commit(s) and merge into current branch, please refer to [this](https://git-scm.com/docs/git-cherry-pick) for more details.

[^4]: **Branch by Abstraction** is a technique for making a large-scale change to a software system in gradual way that allows you to release the system regularly while the change is still in-progress. Please refer to Martin Fowler's [blog](https://www.martinfowler.com/bliki/BranchByAbstraction.html) for more details.

[^5]: **Feature Toggles** (aka Feature Flags), please refer to Martin Fowler's [blog](https://www.martinfowler.com/articles/feature-toggles.html) for more details.


## References

[https://trunkbaseddevelopment.com/](https://trunkbaseddevelopment.com/)

[https://www.thoughtworks.com/continuous-integration](https://www.thoughtworks.com/continuous-integration)

[https://martinfowler.com/bliki/ContinuousDelivery.html](https://martinfowler.com/bliki/ContinuousDelivery.html)

[https://www.martinfowler.com/bliki/BranchByAbstraction.html](https://www.martinfowler.com/bliki/BranchByAbstraction.html)

[https://www.martinfowler.com/articles/feature-toggles.html](https://www.martinfowler.com/articles/feature-toggles.html)

[https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/)

[https://trunkbaseddevelopment.com/alternative-branching-models/](https://trunkbaseddevelopment.com/alternative-branching-models/)

[https://www.thoughtworks.com/insights/blog/enabling-trunk-based-development-deployment-pipelines](https://www.thoughtworks.com/insights/blog/enabling-trunk-based-development-deployment-pipelines)

---

