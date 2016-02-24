---
title: Webpack 学习笔记-2-在文件命中添加hash  
category: webpack  
tags: [webpack, javascript]  
layout: post  

---
### 静态文件版本控制


目前，web开发中静态文件版本控制的方法有两种：

* 通过静态文件的querystring中的版本号来表示不同版本的文件，如`<script type="text/javascript" src="a.js?v=1.1.0"></script>`；
* 通过在文件命中添加hash表示不同版本的文件,如`<script type="text/javascript" src="a_fdsf234d.js"></script>`，或者类似的，通过不同的文件路径区分：`<script type="text/javascript" src="/js/a/fdsf234d/a.js"></script>`

至于为什么，以及怎么做，网上有很多专门的文章，这里就不介绍了。这里仅仅说在项目中怎么用webpack在文件中添加hash。


---

### 文件名hash及引用


#### hash 

添加hash还是非常简单的，如果为bundle文件添加hash，在webpack.config.js的output中的文件名中添加`[hash]`即可，也可以使用同样的方式在路径中添加hash，如：

{% highlight javascript %}
output: {
        path: path.join(__dirname, "assets", "[hash]"),
        publicPath: "assets/[hash]/",
        filename: "output.[hash].bundle.js",
        chunkFilename: "[id].[hash].bundle.js"
    }

{% endhighlight %}

然后下一个问题就是，如何获得hash后的文件名，以便更新html中对该文件的引用。

在这里，我们可以使用`assets-webpack-plugin`这个插件。参考[这里](https://github.com/sporto/)给出的链接，可以很容易的把它添加到webpack中。这样，最终会生成一个assets.json文件，里面包括了无hash的文件名和hash后的文件名之间的一个映射，这样我们就可以知道hash后的文件名并更新html中的引用了。

#### 更新html中的引用
根据具体项目的不同可以有很多办法。我们的项目是ASP.NET MVC项目，用webpack替换掉了ASP.NET MVC提供的bundle库。为了引用hash后的文件名，我创建了一个WebpackHelper的类，通过读取assets.json文件中的mapping，把静态文件的名字添加到cshtml文件中。

### 一个坑

一个似乎都还算顺利，但是，如果你使用了上节中提到的从js中分离css bundle的方法，你就会发现css和js文件的hash是一样的，也就是说即使你只更新的js文件的内容，最终生成的css文件名hash也会更新。

一种解决办法就是不在js中引用css，为css单独创建自己的entry；另一种就是在生成bundle文件之前重新计算hash并用该hash作为文件名。

下一次我们会编写一个webpack plugin来重新计算hash值。
 

