---
layout: page
title: 
---
{% include JB/setup %}

<h3>English：</h3>
<ul class="posts">
  {% for post in site.posts %}
  {% if post.redirection == null and post.lang == 'en' %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
  {% endfor %}
</ul>

<h3>中文：</h3>
<ul class="posts">
  {% for post in site.posts %}
  {% if post.redirection == null and post.lang != 'en' %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
  {% endfor %}
</ul>