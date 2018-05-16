---
layout: page
title:  "My Blog"
section: blog
permalink: /blog/
---

## Blog Posts

{% for post in site.posts %}
  {% if post.title %}
  <h3><a class="page-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h3>
  {% endif %}
{% endfor %}
