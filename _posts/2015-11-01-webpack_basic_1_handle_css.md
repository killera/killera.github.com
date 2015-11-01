---
title: Webpack 学习笔记-1-处理css  
category: webpack  
tags: [webpack, javascript]  
layout: post  

---
### 加载css

Webpack可以直接处理JavaScript文件，但是对于css，image，font等，需要使用loaders将这些资源转换成JavaScript，从而形成一个个模块。

我们继续使用上节的例子，这次我们添加一个`style.css`文件:

{% highlight css %}
body {
    background: yellow;
}
{% endhighlight %}


然后在`entry.js`文件中导入`style.css`，在这里我们先不使用loader，修改`entry.js`内容为：

{% highlight javascript %}
require('./style.css');

document.write(require("./content.js"));
{% endhighlight %}


运行`webpack entry.js bundle.js`, 
生成的bundle文件中函数的参数列表为：
{% highlight javascript %}
/******/ ([
/* 0 */
/***/ function(module, exports, __webpack_require__) {
	__webpack_require__(1);
	document.write(__webpack_require__(2));
/***/ },
/* 1 */
/***/ function(module, exports) {
	body
	{
	    background: yellow;
	}
/***/ },
/* 2 */
/***/ function(module, exports) {
	module.exports = "It works from content.js.";
/***/ }
/******/ ]);
{% endhighlight %}


创建一个HTML文件`index.html`，内容如下：

{% highlight html %}
<html>
    <body>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
{% endhighlight %}

因为没有使用loader，webpack是不知道如何解析css文件的，只是把它封装到了一个模块中，并在使用时试图按照JavaScript脚本来处理。打开控制台，可以看到错误信息：

`Uncaught ReferenceError: body is not defined`

加载一个css文件到底需要几个loader呢？

答案是2个。执行以下命令安装loaders：

`npm install css-loader style-loader`

其中`css-loader`用来处理css文件，`style-loader`用来把css应用到页面上。

修改`entry.js`中的require代码为：

{% highlight javascript %}
require("!style!css!./style.css");
{% endhighlight %}

执行`webpack entry.js bundle.js --display-modules`重新打包，刷新页面，可以看到css已经被应用到页面上。

查看终端输出， 文件大小上升了一个数量级：


{% highlight sh %}
➜  reactjs  webpack entry.js bundle.js
Hash: 48ee99f8f4f8b900ed17
Version: webpack 1.12.1
Time: 41ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.67 kB       0  [emitted]  main
   [0] ./entry.js 66 bytes {0} [built]
   [1] ./style.css 33 bytes {0} [built]
   [2] ./content.js 45 bytes {0} [built]
➜  reactjs  webpack entry.js bundle.js --display-modules
Hash: 6dfb5bc0ecc7bcf179b1
Version: webpack 1.12.1
Time: 422ms
    Asset     Size  Chunks             Chunk Names
bundle.js  10.7 kB       0  [emitted]  main
   [0] ./entry.js 77 bytes {0} [built]
   [1] /Users/xxx/~/style-loader!/Users/xxx/~/css-loader!./style.css 919 bytes {0} [built]
   [2] /Users/xxx/~/css-loader!./style.css 196 bytes {0} [built]
   [3] /Users/xxx/~/css-loader/lib/css-base.js 1.51 kB {0} [built]
   [4] /Users/xxx/~/style-loader/addStyles.js 6.09 kB {0} [built]
   [5] ./content.js 45 bytes {0} [built]
{% endhighlight %}

具体内容这里就不细说了。

`!style!css!`这样的写法叫loader管道。loader在应用时从右到左依次调用。

***

### webpack 配置文件


可以用配置文件来管理webpack的配置。添加`webpack.config.js`文件：

{% highlight javascript %}

module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};

{% endhighlight %}

其中`entry`是要处理的javascript文件，`output`是输出的bundle文件，loader添加在loaders列表中。

这样在`entry.js`文件中就不用添加loader了。

另一种在配置文件中添加loader的方式是：

 ` { test: /\.css$/, loaders: ["style", "css"] }`
 
*** 

### 提取css


通过上述方式，css也会被打包到生成的bundle文件中。如果想把css文件单独打包，可以使用extract-text-webpack-plugin插件。
 
 安装plugin: 
 
 `npm install `,
 
 修改config文件：
 
 {% highlight javascript %}
 var ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader") }
        ]
    },
    plugins: [
        new ExtractTextPlugin("style.bundle.css")
    ]
};
{% endhighlight %}

这样css就会被单独打包到style.bundle.css中了。
 

