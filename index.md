---
layout: page
title:
tagline: 
---
{% include JB/setup %}

    
## Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.
{% highlight bash %}
$ rm -rf _posts/core-samples
{% endhighlight %}

And here's some `random python code`
{% highlight python %}
def init_db():
	'''initiate our db FROM the app rather than the command line - sqlite3 /path/to/file.db < schema.sql -'''
	with closing(hello_db()) as db:
		with app.open_resource('schema.sql') as f:
			db.cursor().executescript(f.read())
		db.commit()
{% endhighlight %}
Here's a sample "posts list".

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This page is a work in progress. Don't look at it. Please, it's hideous. 


