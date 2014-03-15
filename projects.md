---
layout: page
title: "Projects"
description: "List all projects"
---
{% include JB/setup %}


<div class="entries">
    {% for post in site.categories.project %}
  
    <a href="{{ post.url }}">

      <h4 class="post-title">{{ post.title }}</h4>
      <br/>

          
      <img class="port-thumb" src="{{ post.image }}"/> 

    </a>

    {% endfor %}
  </div>
