---
title: Using matplotlib to analyse Locust results
category: test  
tags: [performance test, data visualization]  
layout: post

---

## Forewords

We are using [Locust][locust_website] to do performance test. Locust is a scalable load tesing framework written in python. Locust supplies us two brief reports called request report and distribution report. The reports show us some data about the response time, such as average response time per request, medium response time per reqeust,maximum response time per reqeust, RPS (request per second) and distribution of response time with different percentiles.

The main page of performance testing is like this:

![image][request_chart]
![image][distribution_chart]

We can get some clues from the reports, which help us to do further performance tuning.

But it is not enough. As the performance testing goes on, some performance issues show up. Besides monitoring the server side resource usage, we also want to know how the response trend changes when test running. Thanks the gorgous python 2D plotting library **matplotlib**, we can make any charts we want quickly.

Let's go!



[locust_website]: http://locust.io
[request_chart]:http://localhost:1111 "Locust requests report"
[distribution_chart]:http://localhost:1111 "Locust distribution report"

## Introduction to matplotlib

![image](http://matplotlib.org/_static/logo2.png)
	
>"matplotlib is a python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. matplotlib can be used in python scripts, the python and ipython shell (ala MATLAB®* or Mathematica®†), web application servers, and six graphical user interface toolkits.  
>  
>**--http://matplotlib.org**


Before getting hands dirty, let's follow the convention to get a "hello, world"-like example.

We use the interactive python - ipython to write the code:


	ipython --matplotlib
	
After started ipython, let's type in the following codes:

{% highlight python %}
	# -*- coding:utf-8 -*-

	import matplotlib.pyplot as plt

	x = np.random.randn(1000)
	plt.subplot(221)
	plt.plot(x)
	plt.subplot(222)
	plt.hist(x)
	plt.show()
{% endhighlight %}

We will get two charts like this:

![image](http://killera.github.io/assets/images/matplotlib-sample.png)

The left one is a plotting chart, the right one is histgram chart. At the bottom of the graph, it is an interactive navigation bar, by which you can navigate through the data set.

Easy enough, Aha?

## Generate Locust requests chart

The standard locust requests reports are like this:

![image](http://killera.github.io/assets/images/locust-requests-csv.png)

We can use a bar chart to make the result more readable:

{% highlight python %}
# -*- coding:utf-8 -*-
from optparse import OptionParser
import os
from os.path import basename

import matplotlib.pyplot as plt
import numpy as np


def autolabelh(rects):
    for rect in rects:
        width = rect.get_width()
        plt.text(width * 1.05, rect.get_y() + rect.get_height() / 2., '%d' % int(width),
                 ha='left', va='center', fontdict={'size': 10})


def generate(file_name, img_file_name):
    data = np.genfromtxt(fname=file_name, dtype=None, delimiter=',', names=True, comments=False, autostrip=True)
    headers = data.dtype.names
    name_header, response_header = headers[1], headers[4]
    sorted_data = np.sort(data, order=[response_header])
    name, median_response_time = sorted_data[name_header], sorted_data[response_header]
    bar = plt.barh(range(len(median_response_time)), median_response_time, align='edge', alpha=0.7)
    plt.yticks(range(len(name)), name, ha='right', va='bottom', size='small')
    plt.subplots_adjust(left=0.3)
    plt.grid(True)
    autolabelh(bar)
    title = os.linesep.join([basename(file_name), response_header.replace('_', ' ').upper()])
    plt.suptitle(title, fontsize=12, weight='bold')
    plt.savefig(img_file_name, bbox_inches='tight')


if __name__ == '__main__':
    parser = OptionParser()
    parser.add_option("-s", "--source", dest="csv_file_name",
                      help="read data from csv file", metavar="FILE")

    (options, args) = parser.parse_args()
    img_file_name = '.'.join([options.csv_file_name, 'png'])
    generate(options.csv_file_name, img_file_name)
{% endhighlight %}

#### *Notes:

* We use the numpy lib's `genfromtxt` to read the csv file into 2D array.  
* Use `numpy.sort` to sort the data by the median response time.
* The `barh` method is to generate a horizontal bar chart.
* Use `yticks` method to set the ticks of Y axis with the request names.
* Use `grid` method to add grid into the chart.
* Use `autolabelh` method to add value label beside each bar.
* Use `title` method to add a title, and use `supertitle` method to add a super title (This title will be shared if there are multiple charts.)
* Use `savefig` method to save the chart into a file, the format could be jpg, png, pdf, svg, etc.

The chart would be like this:

![image](http://killera.github.io/assets/images/locust-requests-bar.png)


## Generate Locust request trend chart


## Make it automatically


