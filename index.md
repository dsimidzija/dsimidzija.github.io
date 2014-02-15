---
layout: page
title: Latest posts
#tagline: Supporting tagline
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>
		<h3>
			<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
			&raquo; <span>{{ post.date | date_to_string }}</span>
		<h3>
	</li>
  {% endfor %}
</ul>


