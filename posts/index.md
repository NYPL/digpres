---
layout: page
title: Posts
has_children: True
---

Irregularly published notes based on projects happening within the program.
<ul>
{% for post in site.posts %}
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
    <h2>{{ currentdate }}</h2>
    {% assign date = currentdate %}
  {% endif %}
    <li><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> ({{ post.date | date: "%Y %b %-d" }})</li>
{% endfor %}
</ul>
