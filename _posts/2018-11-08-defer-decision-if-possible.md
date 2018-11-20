---
title: Thinking about Defer decision 
category: development  
tags: [architecture, software development]  
layout: post  
lang: en

---

In Uncle Bob's book <<Clean Architecture>>, he mentioned:

> A good architecture allows you to defer critical decisions, it doesn’t force you to defer them.

He also mentioned:

>..., if you can defer them, it means you have lots of flexibility. Because when you delay a decision, you have more information when it comes time to make it.

"Defer decision" allows you to do **just enough** things, just like simple design, or MVP(Minimum Viable Product). If you decide too many things in advance, the architecture will probably get more and more complex during the development.  

Next, I will take an example in my current project to see if "Defer Decision" makes our life easier and the system more robust.

## Background

There are two applications: **MainApp** and **IntegrationApp**. People plan orders from MainApp, and then synchronise them to IntegrationApp. The orders are stored in different format between these two systems, and we name them `MainAppOrder` and `IntAppOrder`. MainApp uses an sync api to send the orders to IntegrationApp. The payload of the api is defined by IntegrationApp. The process looks like below:

![image](/assets/images/jobqueue0.png)

The IntegrationApp api only only supports single order sync, if the user tries to synced multiple orders to IntegrationApp, those orders will be done one by one.

## Current Design

 People using MainApp tend to do the synchronization for many orders together after they planned orders for a certain period, so the synchronization process may take several minutes to finish. In order not to block the user after they click the 'Sync' button on the page, we introduced Job Queue into the system, and simply create jobs for the orders. During Job creation, we generate the payload according the api contract, and save it into the database. We defined a **Job** table to store all the jobs, the definition looks like:

 ![image](/assets/images/job-table.png)

Then, there is a separate **Job Runner** to execute the jobs and sync to data to IntegrationApp. The workflow looks like:

![image](/assets/images/jobqueue.png)

The **Job Runner** will run every n minutes to sync the data to IntegrationApp. For convenience,the payload we stored in the Job is in the format of the request body that send to IntegrationApp. There are actually 3 applciations in the system now: **MainApp**, **Job Runner**, **IntegrationApp**.

The benefit by storing the payload in database is that we can check the payload send to IntegrationApp api easily. But at the meantime, it brings us some troubles, one of which is that We need to maintain the payload stored in the job. For example, when IntegrationApp api payload changed, and if there are still some jobs with the old payload not executed yet we have to think about how to handle them.

## Re-think with "Defer Decision" 

If we think about the "Defer Decision" here, we can see that actually we don't need to generate the job payload in advance. Instead, we can generate the payload when we are executing the job. As the payload generation requires some basic information(we can call it **metadata**), like the MainAppOrder's OrderId, we need store the **metadata** of payload into the job. 

![image](/assets/images/jobqueue2.png)

In this way, if the IntegrationApp api changed, the Job Runner can easily handle this situation by only changing the payload generation logic.

Some people may ask, "what if the **metadata** need to be changed?" The **metadata** we are talking here is the "**just enough**" information to build the payload. It is more stable compared with the payload itself.

## Wrap up

We discussed a scenario in which the "Defer Decision" strategy is a more suitable choice. In most cases, it makes sense to not rely on the data which are likely to change often, otherwise we will spend more effort to handle with the "temp data" which generated in the process.
