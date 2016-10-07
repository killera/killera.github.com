---

title: Performance testing tools comparison between JMeter and Locust  
category: test  
tags: [JMeter, Locust, performance test]  
layout: post
lang: en
---

## Foreword

Performance test is a very important process in medium and large-scale projects. It is often the one and only way to find the performance issues before release. 

Our team has been using [JMeter][JMeter] to do performance test in one work stream. [Locust][Locust] is a new performance-testing tool, which has been included in [ThoughtWorks' Technology Radar][tw tech radar] in 2012 and 2013. We introduce Locust into our new work stream this year.

In the next, I will give a comprehensive comparison between them.


[tw tech radar]: http://www.thoughtworks.com/radar
[JMeter]: http://jmeter.apache.org/
[Locust]: http://locust.io/

## Overview

Following is an overview of the comparison between JMeter and Locust:  
<br/>  


<table class="table table-bordered table-stripped table-condensed">
<tr><th>      </th> <th>JMeter</th> <th>Locust</th> </tr>

<tr><td>License</td> <td>Apache License 2.0</td> <td>MIT</td> </tr>

<tr> <td>Language</td> <td>Java</td> <td>Python</td> </tr>

<tr> <td>Testing point</td> <td>Http request level</td> <td>Http request Level</td> </tr>

<tr> <td>Maturity</td> <td>★★★</td> <td>★★</td> </tr>

<tr> <td>Concurrency</td> <td>★★</td> <td>★★★</td> </tr>

<tr> <td>Maintainable</td> <td>★</td> <td>★★★</td> </tr>

<tr> <td>Easy learning</td> <td>★★★</td> <td>★★</td> </tr>

<tr> <td>Hackable/Customizable</td> <td>★</td> <td>★★★</td> </tr>

<tr> <td>Multi-platform</td> <td>★★★</td> <td>★★★</td> </tr>

<tr> <td>Scripts Recording</td> <td>Support</td> <td>Not Support</td> </tr>

<tr> <td>Distributed Test</td> <td>★★★</td> <td>★★★</td> </tr>

<tr> <td>Visualization</td> <td>★★</td> <td>★★</td> </tr>

<tr> <td>Easy integrating</td> <td>★★★</td> <td>★★</td> </tr>

<tr> <td>Conducive for designing</td> <td>★</td> <td>★★</td> </tr>


<tfoot>
<tr><td></td><td  colspan="2"> ★ - Low;   ★★ - Medium;    ★★★ - High</td></tr>
</tfoot>
</table>

## Explanation

### License:

JMeter is an open source performance-testing tool developed by Apache.   
Locust is an open source load-testing tool. It is created by several devs who are fed up with some performance tools (including JMeter).

Both JMeter and Locust are open source tools. Their licenses both guarantee end users extreme freedom.

### Language & Multi-platform:

JMeter is implemented in Java, while Locust is implemented in Python. So both can run in multi-platform.

### Testing point:

Both JMeter and Locust focus on the http request level test. That's to say, the HTML page rendering time is not counted in.

### Maturity:

JMeter's latest release is 2.9, it has over 9000 commits, 65 releases, and 2 contributors (gotten from github, the number of committers are 60 in its [Wiki][JMeter Wiki] page).  

Locust's latest release is 0.62, it has about 700 commits, 6 releases, and 19 contributors.  

Locust is a new loading test tool, while JMeter has been used widely.

[JMeter Wiki]: http://wiki.apache.org/jmeter/JMeterCommitters

### Concurrency:

JMeter is thread-based. That's to say, it requires a separated thread to simulate a user.  

Locust is based on co-routine, and uses the async approach. It is possible to simulate large amount of users in one machine.  

JMeter relies more on the machine's performance, and it is more expensive when simulating the same amount of users.

Locust is very helpful in finding the server's problem, such as memory leak, resource exhausted, etc.

### Maintainable:

JMeter generates scripts in one file, and offers a UI interface to presentate the configurations. It is hard to reuse configurations/codes across tests.  

Locust scripts are plain-old python code. The test codes are more reusable, and it is very easy to use version-control tools to track the differences between histories.

As JMeter scripts are recorded. It is exhausting work to filter useless requests such as Javascript request, css requests, and image requests.

### Easy learning:

It is not necessary to learn extra things besides JMeter's usage. It has low requirements to the user.

Locust scripts are Python code. The user should be aware of the basic grammar of Python. As Python is readable and easy to learn, writing Locust test codes is easy.

### Hackable/Customizable:

It is possible to extend JMeter with JavaScript or java plugins, but the overheads are high.

It is easy to hack Locust to support specific functionality. Also, Locust is highly customizable with the help of JavaScripts, Python; and the integration is seamless.

Especially, it is very easy for Locust to combine several requests as one scenario by grouping requests with specific rules.

### Scripts Recording:

JMeter supports script recording, which saves much time when creating the performance test scripts. But, as mentioned before, it costs time to filter noisy requests.

As Locust scripts are Python code, it doesn't support recording scripts.

### Distributed Test:

Both JMeter and Locust supports distributed test, called master-slave mode test.

The slaves' instances are used to simulate large amount of users, while the master instance is used to manage and collect results from slaves.  

It is very convenient to run test in different locations to get user experience in different areas.

### Visualization:

JMeter supplies a UI to display the reports, which generated based on the performance log.

Locust has a neat HTML+JS user interface that shows relevant test details in real-time. Also, it is very convenient for users to start/stop test via UI. 

### Easy integrating:

JMeter has commandline support.

Locust has commandline support for single run mode. For master-slave mode, it is possible to controll Locust via HTTP request.

### Conducive for designing:

It is more conducive for design thinking when writing test code with Locust, such as unnecessary http request, time-consuming sequence call.

For JMeter, as its scripts is recorded, people often pay less attention to it.


## Summary

In comparison, JMeter is more mature than Locust; it is easier to integrate it into CI system. The three major advantages of Locust are concurrency, maintainable and customization. About these 3 parts, there is a detailed introduction in [Locust's features page][Locust Features].

Hope it is helpful for you to choose the most suitable tool according to your needs.

[Locust Features]: http://docs.locust.io/en/latest/what-is-locust.html#features








