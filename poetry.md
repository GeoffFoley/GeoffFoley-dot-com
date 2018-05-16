---
layout: page
title:  "My Poetry"
permalink: /poetry/
section: poetry
---

{% assign poems = site.poetry %}

## Poems

{% if poems.size < 1 %}
  <h3>No poems posted yet.</h3>
{% endif %}

{% for poem in poems %}
  {% if poem.title %}
  <h3><a class="page-link" href="{{ poem.url | prepend: site.baseurl }}">{{ poem.title }}</a></h3>
  {% endif %}
{% endfor %}
