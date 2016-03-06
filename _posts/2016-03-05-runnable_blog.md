---
layout: post  
title: 可以"跑"的博客  
category: python  
tags: [notebook]  

---


<div style='margin:0 auto;width:0px;height:0px;overflow:hidden;'>
<img src="/assets/images/run.png" width='700'>
</div>

这是真的，博客是可以“跑”的，但显然这篇不行。

当然这里说的跑是运行(run)的意思，程序是可以run的，博客也可以。

如果你听说过[IPython Notebook](http://ipython.org/notebook.html)，你就明白我在说什么。

#### IPyton Notebook

IPython Notebook 是一个基于web的交互式的IPython shell环境。你可以在其上编写文档，并可以把它分享给别人。文档中包括丰富的数据表示形式，如HTML、LaTex、SVG、Markdown，当然还要包括Python代码高亮、自动补齐、自动缩进。

Notebook文档中的代码是可以执行的，输出也可以在页面显示出来。这是IPython Notebook 主页的一个示例截图：

![image](http://ipython.org/_static/sloangrant/9_home_fperez_prof_grants_1207-sloan-ipython_proposal_fig_ipython-notebook-specgram.png)


#### Jupyter

当然，如果只能跑Python代码，似乎也没有那么大的吸引力，于是[Jupyter](https://jupyter.org/)诞生了。
![image](https://jupyter.org/assets/main-logo.svg)。Jupyter是IPython和Python解释器剥离后的产物，可以和其他的编程语言结合来提供服务。你可以在Jupyter中运行超过**40**种编程语言。

Jupyter有三种用户界面：终端、Notebook和Qt终端，我们今天要说的是Jupyter Notebook。

通过提供不同编程语言的核，Jupyter可以运行不同的程序。
[这里](https://github.com/ipython/ipython/wiki/IPython-kernels-for-other-languages)是一些编程语言核的列表，julia、js、c#、java、ruby、scala、go等等都在其列。

这里是jupyter的一个demo界面，提供了几种不同语言的noteboot文件（以.ipynb为后缀名）如Julia、R、Haskell、Python和Scala，你可以在线尝试一下。

![image]({{ site.baseurl }}/notebooks/jupyter.png)


#### github & nbviewer

Jupyter Notebook文件是以json形式保存的。Github上可以直接预览，非常方便。

nbviewer提供了更好的渲染服务，把github中文件的路径粘贴到[http://nbviewer.jupyter.org/](http://nbviewer.jupyter.org/)页面的输入框中，就可以看到一个更加美观的页面。

[这里](http://nbviewer.jupyter.org/github/qszhuan/qszhuan-notebook/blob/master/data-visualization/Matplotlib%20plotting%20styles.ipynb)是我用jupyter写的一篇关于matplotlib的文章。


如果你只是想写点东西，借助jupyter也是再好不过了，只需要创建一个代码库，把jupyter文件传上去，然后在nbviewer中输入代码库地址，一个博客网站就诞生了。











