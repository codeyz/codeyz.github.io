---
layout: default
---
{% include JB/setup %}

{% assign post = site.posts.first %}
{% assign content = post.content %}
{% assign date = post.date %}
{% assign title = post.title %}
{% assign tagline = post.tagline %}
{% assign tags = post.tags %}
{% assign categories = post.categories %}
{% assign previous = post.previous %}
{% assign previous.title = post.previous.title %}
{% assign previous.url = post.previous.url %}
{% assign next = post.next %}
{% assign next.title = post.next.title %}
{% assign next.url = post.next.url %}

{% include themes/bootstrap-3/homepage.html %}
