---
title: Webpack 学习笔记-0  
category: webpack  
tags: [webpack, javascript]  
layout: post  

---

一个月前，在一个node + react项目中，我们使用了[webpack](https://webpack.github.io/)作为js和css的打包工具。总的说来还是不错的，比较容易上手和使用。但是由于时间比较紧，没有花太多的时间仔细研究webpack。在正在进行的另一个项目中，我们已经决定继续尝试webpack + react，并且有打算用webpack替换掉ASP.NET Bundles，所以正好有时间好好学习总结一下：

## 安装
首先需要安装[node.js](https://nodejs.org/en/),然后运行命令：
`npm install webpack -g`
 
## 示例
在此，我们使用webpack官网的例子：

创建entry.js:

{% highlight javascript %}
document.write("It works.");
{% endhighlight %}


然后执行 `webpack entry.js bundle.js`，便会生成一个编译好的bundle文件,我们不妨打开这个文件看一下里面有什么(使用的webpack版本为1.12.1)：

{% highlight javascript %}

 (function(modules) { // webpackBootstrap
 	// The module cache
 	var installedModules = {};

 	// The require function
 	function __webpack_require__(moduleId) {

 		// Check if module is in cache
 		if(installedModules[moduleId])
 			return installedModules[moduleId].exports;

 		// Create a new module (and put it into the cache)
 		var module = installedModules[moduleId] = {
 			exports: {},
 			id: moduleId,
 			loaded: false
 		};

 		// Execute the module function
 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

 		// Flag the module as loaded
 		module.loaded = true;

 		// Return the exports of the module
 		return module.exports;
 	}


 	// expose the modules object (__webpack_modules__)
 	__webpack_require__.m = modules;

 	// expose the module cache
 	__webpack_require__.c = installedModules;

 	// __webpack_public_path__
 	__webpack_require__.p = "";

 	// Load entry module and return exports
 	return __webpack_require__(0);
 })
/************************************************************************/
 ([
/* 0 */
/***/ function(module, exports) {

	document.write('It works.');


/***/ }
 ]);
 
{% endhighlight %}
 
屏蔽掉里面的一些具体细节，可以看到实际上最外层是在执行一个function，传入的参数是我们在一个function的数组：
 
 {% highlight javascript %}
 
  (function(modules) { // webpackBootstrap
 	// The module cache
 	var installedModules = {};
 	// The require function
 	function __webpack_require__(moduleId) {
 		......
 	}
 	......
 	// Load entry module and return exports
 	return __webpack_require__(0);
 })
  ([
/* 0 */
/***/ function(module, exports) {
	document.write('It works.');
/***/ }
 ]);
 
 {% endhighlight %}

因为我们只打包了一个entry.js文件，所以数组长度为1。

通过注释`/* 0 */`我们可以清楚的看出是第几个module（即数组中的第几个）。

通过这种方式，webpack把javascript打包到一个bundle文件中，并暴露原javascript文件的内容。

原理很简单，是吧。

### 依赖

下面，我们在`entry.js`中引入另一个`content.js`文件，内容为：

 {% highlight javascript %}
 module.exports = "It works from content.js.";
 {% endhighlight %}
 
 修改原`entry.js`文件内容为：
 
 {% highlight javascript %}
 document.write(require("./content.js"));
 {% endhighlight %}
 
 然后执行`webpack entry.js bundle.js`再看结果：
 
 {% highlight javascript %}
 document.write(require("./content.js"));
 {% endhighlight %}
 
 
 编译后的bundle文件，前面的部分是一样一样的，不同的只是传入的模块列表：
 
{% highlight javascript %}
 /************************************************************************/
/******/ ([
/* 0 */
/***/ function(module, exports, __webpack_require__) {

	document.write(__webpack_require__(1));

/***/ },
/* 1 */
/***/ function(module, exports) {

	module.exports = "It works from content.js.";

/***/ }
/******/ ]);

 {% endhighlight %}
 
 由此可见，webpack把javascript文件都封装成一个个module，然后编译到一个bundle文件中。正是借助这种思想，webpack不仅限于处理javascript文件，还可以处理css、img、font等文件。如果想用React来编写可重用的components，webpack无疑提供了很大的便利。
 
