---
layout: page
title: "Blog"
permalink: /blog/
theme: jekyll-theme-minimal
---

## Blog

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }}) – {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
