---
title: 使用ts的outDir参数  
category: javascript  
tags: [ts]  
layout: post  

---

在按照Angular2官网用ts写app时，默认下ts的编译文件输出路径和原ts文件相同，可以在tsconfig.json文件中添加`“outDir”: "build"`来改变输出路径到build目录，或者在package.json文件中修改` "tsc": "tsc --outDir build",` 。

 然后修改`index.html`文件中SystemJs代码：
 
 `System.import('build/main')`
 

添加完成后，发现报错:

![image](/assets/images/ts-outdir-error.png)


原因是没有更新SystemJs的packages配置：

{% highlight javascript %}
<script>
      System.config({
        packages: {        
          app: {
            format: 'register',
            defaultExtension: 'js'
          }
        }
      });
      System.import('build/main')
            .then(null, console.error.bind(console));
</script>
{% endhighlight %}


将`packages`中的`app`改为`build`就可以了。