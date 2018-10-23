---
title: Performance tuning Notes
category: development  
tags: [sql, dot net, performance tuning, sql server]  
layout: post  
lang: en
---


I made several performance tuning these days, and keep it here for further check.

## Before start

* Use similiar scale data
* Know about the user's behaviour
* Use tools to locate the bottleneck/issue, don't make assumption.
* Validate often
* Check server performance first, if not good, upgrade


## Database Related

* Select just enough data only
* Beware the Scalar Function
* Avoid N+1 Select
* COMPATIBILITY_LEVEL


## Code Related

* Move expensive filters out of loop
* Dictionary over List 
* compare string.toupper() as dictioanry key, or make dictionary stringcompare.IgnoreCase
