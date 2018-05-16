---
layout: page
title:  "My Fiction"
section: fiction
permalink: /fiction/
---

{% assign stories = site.fiction %}

## Stories

{% if stories.size < 1 %}
  <h3>No stories posted yet.</h3>
{% endif %}

{% for story in stories %}
  {% if story.title %}
  <h3><a class="page-link" href="{{ story.url | prepend: site.baseurl }}">{{ story.title }}</a></h3>
  {% endif %}
{% endfor %}
