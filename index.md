---
layout: default
title: Home
---

A collection of my thoughts.

### Posts
{% for post in site.posts %}
- **{{ post.date | date: "%Y-%m-%d" }}** - [{{ post.title }}]({{ post.url | relative_url }})
{% else %}
- No posts found yet!
{% endfor %}
