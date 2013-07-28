---
layout: page
title: Welcome to KillerA's Blog!
tagline: Coding for fun
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {{site.post[0].content}}
  {% endfor %}
</ul>
