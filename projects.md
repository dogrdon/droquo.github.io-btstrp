---
layout: page
title: "Projects"
description: "List all projects"
---
{% include JB/setup %}

<div class="entries">
    {% for post in site.JB.projects %}

    <a href="{{ post.url }}">

      <h4 class="post-title">{{ post.title }}</h4>
      <br/>
      <!-- <div class="entry-container"> -->
      

        
          
        <!--   
          <div class="port-thumb"> -->
            <img class="port-thumb" src="{{ post.image }}"></img> 

          <!-- </div>  -->      
      
      <!-- </div> -->
    </a>

    {% endfor %}
  </div>
