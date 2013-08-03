---
title: Performance testing, the missing link in software delivery  
category: test  
tags: [performance test, continuous delivery]  
layout: post  

---


## Forword

No doubt, performance testing is an important stage in the software delivery. In the book __Continuous Delivery__[^1], there is one chapter talking about the _NonFunctional requirement testing_. It mentioned the performance testing briefly.

But there is always a gap between reality and ideality. In most projects, the performance testing is treated as a seperated waterfall stage before realse. It is always operated by a seperated group of people. Sometimes, people just neglected it, and didn't schedule enought time for it. So the performance testing is often performed along with the relase.


[^1]: Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation Jez Humble (Author), David Farley (Author)

## Where is the promised 'working software'?

It is close to the release deadline. The features are almost finished, and team members have the confidence to finish all remaining part two weeks before the release. In the last two weeks, QAs can do some mannual test to see if there are still any defects so that Devs can quickly fix it. Everything seems under control. 

But at the moment, the client expresses his concern, the system is very slow, it is unacceptable. They can not deliver it only when the performance problem solved.

Everyone gets busy, team members start to fix the problem according to the client's feedback. A group of people start to do performance test to check if there are still other pages having the performance problem. A few people remain to fix the other defects.

Chin up! Let's finish the 'Last Mile'.

## Treat performance testing as a first-class citizen

Attitude is everything.

Usually, people treat performance testing as a second-class citizen. They think of it only when the release is close, or only when the performance issue exposes.

Usually, people think that the performance testing is one-off. Do it when need, drop it after done. 

So they spend much time to build continuous integration environment, working on the features to make it correct. They think it is more important to make software correct rather than to make it faster. What's worse, they think it is a toil, and they even don't want to touch it. 

Working software is far more than correct software. The performance is an unavoidable topic. Without performance, the work is still uncompleted. Sometimes, a bad performance declares the closing of the project.  

Performance testing could help us to find many problems, such as memory leak, inproper system parameter configuration, defect under concurrency, and etc.

Performance tuning is a challenging work, it requires more knowlege besides the software development. 


## When should we do performance testing?

It is too late to introduce performance test just before the realse. So when to introduce it?

>premature optimization is the root of all evil. -Donald Knuth

Maybe this is the motivation that many people postpone the performance testing. 

Is it the excuse to neglect or postpone performance test? Let's read the whole paragraph:

>Programmers waste enormous amounts of time thinking about, or worrying about, the speed of noncritical parts of their programs, and these attempts at efficiency actually have a strong negative impact when debugging and maintenance are considered. We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.

I think we should do performance testing along with the developing work. Considering performance in feature developing stage is a little earlier. But after one story or one feature done, it is the time to write performance testing scripts to verify whether the performance is acceptable or not.

If we do performance testing against several featues together. we should always keep in mind that it will slow down the process of the deployment.

Also, performance testing is a long-term work. We usually do performance testing aimed at the user amount at that moment. As time goes on, we need to ensure the system's is still ok after the number of users increases and new features added.

## Who will do performance testing?

There are two parts of performance testing.
One is to write scripts to see the current system's performance. The other is to do performance tuning.

There shouldn't be a seperated team to write testing scripts and do performance tuning. Because the people who worked on the feature have more context. If we build another special team for the performance testing, they need to spend more time to figure out what's the system's behavior, it is a waste. For the lack of context, they also need times to figure out the dataset needed for performance test.

So it is reasonable to keep feature implementation and performance testing in one team.

## What do we need to do for performance testing?

Before doing performance testing, we need to know the target. One is the number of users, the other is the acceptable performance criteria.

Often the client knows clear about how much users the system supported. Even the client do not know the exact number, he can help to give a prospective one. 

For the second target, say performance criteria. It depends on the user's experience. It is also possible to dicsuss with client to get a number first.

Also, a production-like envrioment is necessary, including the similiar server configuration, similiar network condition, and silimiar background data.

Besides that, it is also necessary to set up some performance monitoring tools to collect performance data (such as server CPU usage, memory usage, SQL quires). After a round of performance test, it will help us a lot to find the bottle neck and give us the direction to do performance tuning. Some tools, such as New Relic, AppDynamic can supply many informations for us.

The network latency is different a lot for different regions. Runing the performance test in the target user's location will make us focus more on the server response time. 

Although most performance tool only help to find out the server response time, but it is not all the things. Another part of performance is the page render time (For website application). We also need to find out a way to test it.


## Performance is not only about 'Page Loading Time'

The performance is good or not depends on the real users' experience. Able to response fast is not enough. 

Another question is "do the users consider the system easy to use?". A good application should make the user comfortable, saving their time, and make their life easier.

So when we do performance test, it is important to keep in mind that whether the current system's workflow is good or not.


## Conclusion

Continous integration (CI) is already not something now in nowaday's software development. It has been accepted widly as a excellent software development process. Continous delivery (CD) also becomes the aim we are pursuing. So, do not make performance testing the Last Mile in delivering working software.

I highly recommend the book "__The ThoughtWorks Anthology, Volume 2__"[^2] to you. There is one whole chapter elaberating the performance testing. Many views in the book is consistent with what our team learned from work. I really wish I have read the book before we do performance testing.  

[^2]: http://shop.oreilly.com/product/9781937785000.do
