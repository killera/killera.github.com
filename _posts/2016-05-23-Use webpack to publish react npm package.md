---
title: Use webpack to publish reactjs npm package  
category: javascript  
tags: [npm, webpack, component, react]  
layout: post  
thumbnail: /assets/images/books/component.jpg
---

React is a very easy and light-weight way to build componentized website. Sometimes we need to share the components among projects, build component and publish it to npm repository is the most convinient way to share and maintain.

The next will cover:

* Set up private npm server 
* Write a 'HelloMessage' React Component 
* Publish npm package & configuration

### Local npm server

If you want to keep the components private, one way is to use the private function in npm, the other is to set a local(or private) npm server.

It's very easy to set up a private npm server with [sinopia](https://www.npmjs.com/package/sinopia#installation).

Copy the Installation scripts here from it's page:

{% highlight sh %}
#installation and starting (application will create default
# config in config.yaml you can edit later)
$ npm install -g sinopia
$ sinopia
 
# npm configuration
$ npm set registry http://localhost:4873/

{% endhighlight %}

Then, access [http://localhost:4873/](http://localhost:4873/) to see the local npm server.


### Write A HelloMessage Component.

We want to keep the component as simple as possible.

{% highlight sh %}

# create root folder
mkdir component

# create folder for component 'HelloMessage'
cd component
npm init
pm install react react-dom jsx-loader --save

mkdir hello
touch hello.jsx

# init package
npm init

# install react
npm install react jsx-loader --save

{% endhighlight %}

Implement the HelloMessage Component:

{% highlight javascript %}
'use strict'
var React = require('react');

module.exports = React.createClass({
    displayName: 'HelloMessage',
    render: function(){
        return <div className="hello">
        (Hello React.
        {this.props.children}
        )
        </div>
    }
})
{% endhighlight %}

Then, goto the root folder to add index.html, loader.jsx, and webpack.config.js.

This is the loader.jsx:

{% highlight javascript %}
var HelloMessage = require('./hello/hello.jsx');

 ReactDOM.render(
        <HelloMessage >
        <div>this is from loader</div>
        </HelloMessage>,
        document.getElementById('content')
      );
{% endhighlight %}

This is the index.html:

{% highlight html %}
<html>
    <head>
        <title>Component Container</title>
         <script src="./node_modules/react/dist/react.js"></script>
        <script src="./node_modules/react-dom/dist/react-dom.js"></script>
    </head>
    <body>
        <div id="content">
            This is the container.
        </div>
       
        <script src="./dist/loader.js"></script>
    </body>
</html>
{% endhighlight %}


Next, we need to add webpack to build bundle file and put it in dist folder.

Here is the webpack.config.js:

{% highlight javascript %}
var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'dist');
var APP_DIR = path.resolve(__dirname, '.');

var config = {
  devtool:'source-map',
  entry: APP_DIR + '/loader.jsx',
  output: {
    path: BUILD_DIR,
    filename: 'loader.js'
  },
  module : {
     loaders: [
            { test: /\.jsx$/, loader: "jsx-loader" },
        ],
  }
};

module.exports = config;
{% endhighlight %}

Make sure `webpack`, `jsx-loader`, `react` and `react-dom` installed.

Run `webpack` under root folder to build bundle file.



Start a local server to check the result:

`python -m SimpleHTTPServer`(or run `python -m http.server` if you are using python3)


Then, visit [http://localhost:8000](http://localhost:8000)


### Publish to local npm server

Actually, if we run `npm publish` under hello folder, we will be able to see it in npm repo.

This is just pack all the files under hello folder into the npm package.

There are several things we need to consider:

#### The entry
	
It's the `main` config in pacakge.json file, the value was set when running `npm init`, in our example, it should be set to `hello.jsx`	
Sometimes, it's not a good choice to expose a 'jsx' entry file, as you don't really want to look into the npm packages folder to decide what loaders do you need to use to import this module.
	
We can use `babel` to compile the jsx files into ES5.

Goto the `hello` folder to install babel-cli:
	
`npm install --save-dev babel-cli babel-preset-es2015 babel-preset-react`
	
Then, add the following to package.json file:
	
{% highlight javascript %}
  "main": "index.js",
  "scripts": {
    ...
    "build": "babel hello.jsx -o hello.js"
  },
  "babel": {
    "presets": [
      "es2015",
      "react"
    ]
  }
  {% endhighlight %}
  
Run `npm run build` to compile it.
	
#### An alternative way to build the component

In some project, react is just used to build some part of one page. In order to simplify the use of component, we can build it as a library(like jquery) to expose a global variable.
	
Add this webpack.config.js into hello folder:
	
{% highlight javascript %}
var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'dist');
var APP_DIR = path.resolve(__dirname, '.');

var config = {
  devtool:'source-map',
  entry: APP_DIR + '/hello.jsx',
  output: {
    path: BUILD_DIR,
    filename: 'hello.lib.js',
    library:'HelloMessage',
    libraryTarget:'var'
  },
  module : {
     loaders: [
            { test: /\.jsx$/, loader: "jsx-loader" },
        ],
  }
};

module.exports = config;
{% endhighlight %}
	
After run `webpack`, we got a `hello.lib.js` file in dist folder.
   
Change the index.html file, replace 

`
<script src="./dist/loader.js"></script>
`

with 
   
{% highlight javascript %}
<script src="./hello/dist/hello.lib.js"></script>
<script>
    ReactDOM.render(React.createElement(HelloMessage),
                document.getElementById('content'));
</script>
{% endhighlight %}

In this way, we got rid of the `loader.jsx` file.


The sample code can be found here:

[https://github.com/qszhuan/npm-webpack-react-component-sample/tree/basic](https://github.com/qszhuan/npm-webpack-react-component-sample/tree/basic)
	
	