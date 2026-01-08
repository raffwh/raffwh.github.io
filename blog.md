---
layout: posts
title: "Blog"
permalink: /blog/
author_profile: true
---
## Blog

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }}) – {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
