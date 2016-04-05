---
layout: page
title: 
---
{% include JB/setup %}


<div class="recent-posts">
  {% for post in site.posts limit: 3 %}
      <article class="post">
        <h3>
            <a href="{{ post.url }}">{{ post.title }}</a>
        </h3>

        <div class="entry">
          {{ post.content }}
        </div>
        <div>
        {% unless post.categories == empty %}
            <ul class="tag_box inline">
              <li>
                <i class="icon-folder-open"></i> 
                <a href="{{ BASE_PATH }}{{ site.JB.categories_path }}#{{ post.category }}-ref">
    		        {{ post.category | join: "/" }}
    	        </a>
                <span class="date"> {{ post.date | date: "%B %e, %Y" }} </span>

    	      </li>
            </ul>

        {% endunless %}  
        </div>
      
      </article>
  {% endfor %}
</div>