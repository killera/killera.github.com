---
title: Add CSP to github pages  
category: technology  
tags: [security, csp]  
layout: post  

---


> "...Content Security Policy, a mechanism web applications can use to mitigate a broad class of content injection vulnerabilities, such as cross-site scripting (XSS). Content Security Policy is a declarative policy that lets the authors (or server administrators) of a web application inform the client about the sources from which the application expects to load resources..."     
--http://www.w3.org/TR/CSP/

CSP(Content Security Policy) shows us a new way to mitigate XSS. This can be done by adding a header `Content-Security-Policy` in the website simply.

There are several directives, such as `script-src`, `style-src`, etc, which can be used to control different file types.

For example, if you do not want to load javascript file from other sites, you can write like `script-src 'self' 'unsafe-inline';` , in which 'self' indicates that the javascript files from the same domain are allowed, and 'unsafe-inline' indicates that inline javascript is allowed.

For more details, you can read the documentations(http://www.w3.org/TR/CSP/).

There is an online [website](https://report-uri.io/home/generate), which you can use to generate CSP.

So, if we want to add CSP to web application, we can config the server to add the CSP header. But for the blogs hosted in github pages, how can we do?

The answer is to use `<META HTTP-EQUIV="...">`. By doing this, the browser will generally use HTTP-EQUIV as if it were an HTTP header, but it is not really made into header.

So what we can do is add it to a html `head.html`, it might look like this:

{% highlight html %}
`<META HTTP-EQUIV='Content-Security-Policy' CONTENT="default-src 'self' ; script-src 'self' 'unsafe-inline' *.disqus.com a.disquscdn.com requirejs.org www.google-analytics.com; style-src 'self' 'unsafe-inline' a.disquscdn.com; img-src 'self' *; media-src 'self' ; frame-src disqus.com;">`
{% endhighlight %}

And then, include this `head.html` file in every page(or a template html file):
{% raw %}
`{% include head.html %}`
{% endraw %}







