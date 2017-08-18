---
title: Elastic Beanstalk notes 
category: aws  
tags: [aws,eb]  
layout: post  
lang: en
---

[eb tutorial for python flask app](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-flask.html)
[eb tutorial for .net core](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/dotnet-core-tutorial.html)


Notes: 
1. You can directly launch a sample application that aws made for you. but if you want to remote to that server later, don't choose this option. As there is no way to let you assign **EC2 Key Pair**
![image](/assets/images/eb.sample.launch.png)
2. If you try to deploy a python app, it will failed because the pip version is pretty old. you have to upgrade it. Steps are:
    * add `.ebextensions` folder on the root folder of your app;
    * add a config file inside it, may name as `pip.config`, the content is :

        ```
        commands:
            update_pip:
                command: "/opt/python/run/venv/bin/pip install --upgrade pip"
        ```
    * run `eb deploy` again, it will be resolved.