---
layout: page
title: ![title](http://www.dan-dare.org/FreeFun/Images/CartoonsMoviesTV/ShrekWallpaper800.jpg)
tagline: 
category: home
---
{% include JB/setup %}

![spinning pentakisdodechahedron on wikipedia](http://upload.wikimedia.org/wikipedia/commons/f/fc/Pentakisdodecahedron02.gif "Spinning Pentakisdodecahedron")

##Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This page is a work in progress. Don't look at it. Please, it's hideous. 


