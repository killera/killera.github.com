---
title: Integrate TeamCity with Git Flow  
category: git  
tags: [git, git flow, TeamCity, CI]  
layout: post  
lang: en
---

**Git Flow** is  a very strict branching model, which is often used in projects that adpoting the `feature branches` strategy. 
It provides some command lines to help the developer to save time in switching on/off branches. The branch names follow a set of convention, such as `feature/***` for feature branches, `bugfix/***` for bug fix branches, `develop` as the trunk branch and `master` as the production branch.  It's very easy to config the CI tools to monitoring the branch changes, and kick off the `build-test-package` process. We use `TeamCity` as the CI tool to explain how to configure CI pipelines for different kinds of branches.

## Configure pipelines 

### Configure pipeline for  Master

* Create a project in TeamCity, and configure the repo Url.

![image](/assets/images/tc-create-project.jpg)

* Configure the project name as `TeamCity GitFlow`,  and set build configuration name to `Master`. Now we have a new project with a build for the `master` branch changes.

![image](/assets/images/tc-name-project.PNG)

### Configure pipeline for Develop

* Copy the Master configuration to create another one for `develop` branch.

![image](/assets/images/tc-create-develop.PNG)

Set the names as follow:

![image](/assets/images/tc-copy-project.PNG)

Make sure they have different VCS roots:

![image](/assets/images/tc-vcs-root.PNG)

### Configura pipelines for features and releases

* Then create `Features-Bugfixes` build configuration by coping the `Develop` configuration; create `Releases-Hotfixes` build configuration by coping the `Master` configuration. We will have 4 build configurations as below. 

![image](/assets/images/tc-all-configurations.PNG)

Now the `Features-Bugfixes` and `Develop` share the same VCS root; `Releases-Hotfixes` and `Master` share the same VCS root. 

* Edit the vcs root for  `Features-Bugfixes`. Change the `VCS root name` to `https://github.com/qszhuan/teamcity-gitflow-integration.git#refs/heads/features-bugfixes`, and edit the Branch specification as below. When click `Save`, there are two options: create a separate one or override the current one as it's used in two places(`Features-Bugfixes` and `Develop`). choose to create a new one.

![image](/assets/images/tc-vcs-root-feature.PNG)


* Edit the vcs root for  `Releases-Hotfixes`. Change the `VCS root name` to `https://github.com/qszhuan/teamcity-gitflow-integration.git#refs/heads/features-bugfixes`, and edit the Branch specification as below. Finally we will get 4 vcs roots.

![image](/assets/images/tc-vcs-root-release.PNG)

### Disable default branch build for features and releases

* The last step is to change the `Version Control Settings` to untick the `Allow builds in the default branch` option for `Features-Bugfixes` and `Releases-Hotfixes`.


### Test and see the result

Now it's done.
Try to create feature branches or bugfix branches to check the CI pipelines. This is one sample:

![image](/assets/images/tc-gitflow-pipelines.PNG)


## Version Control settings

Now we have a skeleton config for Git flow. we can save the project configuration into a repo. so that we can reuse it to generate settings for other projects.

It's better to create a separate repo to store the teamcity settings. If you are using TeamCity 2017 or later, you can store 

![image](/assets/images/tc-version-control.PNG)



