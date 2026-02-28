---
layout: page
title: Algorithm
permalink: /algorithm/
---

Here is the landing pages for algorithms

<ul>
  {% for post in site.categories["algorithms"] %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>