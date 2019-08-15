---
layout: page  
title: 中文  
header : Book Readings  
group: navigation  
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
  {% if post.redirection == null and post.lang != 'en' %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
  {% endfor %}
</ul>