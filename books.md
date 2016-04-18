---
layout: page
title : 读书
header : Book Readings
group: navigation
---
{% include JB/setup %}

<div class="readings">
  {% for post in site.posts %}

    {% if post.category == "reading" %}
      <article class="post">
        <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
        <div class="entry">
          <div  class="thumbnails">
            <img src="{{post.thumbnail}}" width='200'>
          </div>
          <div class="intro">
          {{ post.content | strip_html | truncatewords:5 }}
          </div>
        </div>

        <a href="{{ post.url }}" class="read-more">Read More >> </a>
      </article>
    {% endif %}
  {% endfor %}
</div>