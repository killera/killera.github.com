---
layout: page
title: 

---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
  {% if post.redirection == null and post.lang == 'en' %}
    <li><span class="date">{{ post.date | date_to_string }}</span> <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
  {% endfor %}
</ul>

