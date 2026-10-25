---
layout: default
title: Home
---

<p class="home-description">A collection of my thoughts, among other stuff.</p>

## Posts

{% for post in site.posts %}
<div class="post-entry">
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
</div>
{% else %}
<p class="muted">No posts yet.</p>
{% endfor %}