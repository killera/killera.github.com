---
title: Use webpack to publish reactjs npm package  
category: javascript  
tags: [npm, webpack, component, react]  
layout: post  
thumbnail: /assets/images/books/component.jpg
---

React is a very easy and light-weight way to build componentized website. Sometimes we need to share the components among projects, build component and publish it to npm repository is the most convinient way to share and maintain.

### Local npm server

If you want to keep the components private, one way is to use the private function in npm, the other is to set a local(or private) npm server.

It's very easy to set up a private npm server with [sinopia](https://www.npmjs.com/package/sinopia#installation).

Copy the Installation scripts here from it's page:

{% highlight sh %}

# installation and starting (application will create default
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


Start a local server to check the result.

`python -m SimpleHTTPServer`

then, visit [http://localhost:8000](http://localhost:8000)


### Publish to local npm server

Actually, if we run `npm publish` under hello folder, we will be able to see it in npm repo.

This is just pack all the files under hello folder into the npm package.


###

To be continue.....
 
