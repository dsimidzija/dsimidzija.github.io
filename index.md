---
layout: page
title: You are here
tagline: Welcome to the website you're looking at
---
{% include JB/setup %}

# Welcome

This is the place where I write about programming, linux, technology and
whatever comes to my mind.

# Latest post

<div class="posts">
	{% for post in site.posts limit:1 %}
		<div>
			<h2>
				<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
				<span style="font-size: 0.7em;">&raquo; {{ post.date | date: "%Y-%m-%d" }}</span>
			</h2>
			<p>{{ post.excerpt }}</p>
		</div>
	{% endfor %}
</div>

# Recent posts

<div class="posts">
	{% for post in site.posts limit:10 offset:1 %}
		<div>
			<h2>
				<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
				<span style="font-size: 0.7em;">&raquo; {{ post.date | date: "%Y-%m-%d" }}</span>
			</h2>
			<p>{{ post.excerpt }}</p>
		</div>
	{% endfor %}
</div>
