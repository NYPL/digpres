---
layout: page
title: Topics
redirect_from: /topics/
parent: Posts
---

Here are all the posts on this site grouped by topic.

{% for category in site.categories %}
  <h3>{{ category | first | capitalize }}</h3>
  {% for item in category %}
  <ul>
    {% for post in item  limit:3 %}
      {% if post.title %}
      <li><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> ({{ post.date | date: "%Y %b %-d" }})</li>
      {% endif %}
    {% endfor %}
  </ul>
  {% endfor %}
{% endfor %}
