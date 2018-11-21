---
title: Thinking about "Defer decision"  
category: development  
tags: [architecture, software development]  
layout: post  
lang: en

---

In Uncle Bob's book &lt;&lt; Clean Architecture &gt;&gt;, he mentioned:

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

[Note:] In the diagram, all the sync steps happen after the Job Creation steps finished.

The **Job Runner** will run every `n` minutes to sync the data to IntegrationApp. For convenience,the payload we stored in the Job is in the format of the request body that send to IntegrationApp. There are actually 3 applciations in the system now: **MainApp**, **Job Runner**, **IntegrationApp**.

The benefit by storing the payload in database is that we can check the payload send to IntegrationApp api easily. But at the meantime, it brings us some troubles, one of which is that We need to maintain the payload stored in the job. For example, when IntegrationApp api payload changed, and if there are still some jobs with the old payload not executed yet we have to think about how to handle them.

## Re-think with "Defer Decision" 

If we think about the "Defer Decision" here, we can see that actually we don't need to generate the job payload in advance. Instead, we can generate the payload when we are executing the job. As the payload generation requires some basic information(we can call it **metadata**), like the MainAppOrder's OrderId, we need store the **metadata** of payload into the job. 

![image](/assets/images/jobqueue2.png)

In this way, if the IntegrationApp api changed, the Job Runner can easily handle this situation by only changing the payload generation logic.

Some people may ask, "what if the **metadata** need to be changed?" The **metadata** we are talking here is the "**just enough**" information to build the payload. It is more stable compared with the payload itself.

## Wrap up

We discussed a scenario in which the "Defer Decision" strategy is a more suitable choice. In most cases, it makes sense to not rely on the data which are likely to change often, otherwise we will spend more effort to handle with the "temp data" which generated in the process.


---

## Follow up

### Another supporter
Days later, when I work on a story related to the above process, I found that there is an IntegrationApp api call when generating the Job and payload. The reason is that some data required in the payload are not in MainApp system.

So the work flow becomes:


![image](/assets/images/jobqueue3.png)

From the diagram, we can see that there is a dependency during the Job creation process. If the api call failed, the job won't be created.   

The diagram here is a simple version of the real application, actually the Job creation process is within `MainAppOrder Approval` process, which means that the `MainAppOrder Approval` process will fail if the Job creation failed.   

This is not a necessary restriction. If we only store the metadata in Job, and generate the payload just before sending it to IntegrationApp, the MainApp users can still approve orders successfully. The Job Runner will take care the payload generation and retry logic if any failure happened. 

### Some discussion

I had a deep discussion with my colleague about deferring the payload generation. The key points are:

1. Job Runner can be a generic service, which should not care about the payload generation.
2. Generating and keeping payload in Job table is more convenient for supporting. 
3. Maintaining the metadata also needs effort.

For **2**, after payload after generation, we can still store it in Job table for auditing or supporting. So there are two columns in Job table: column `Metadata` and column `Payload`, and the `Payload` column is only set during the sync process.

For **1**, I agree that some Job Runner can be independent, like sending email. But some Job Runners have some other operations after Job executed(like Step <span style="text-decoration-line: underline;">**5**</span> to update MainAppOrder status). In this case, the dependency is necessary. In step <span style="text-decoration-line: underline;">**5**</span>, the Job Runner will call an api in MainApp to update the MainAppOrder status to synced, which is a good way to reduce the dependency by introducing api call.

For **3**, this is the main point discussed in this blog. Generally speaking, the metadata is more stable than the payload generated with it. We defer the decision to gain more flexibility and save effort.

All in all, there is no best, only better. The important thing is to choose the one fit.
