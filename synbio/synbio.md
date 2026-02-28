---
layout: page
title: SynBio
permalink: /synbio/
---

Here is the landing pages for synthetic biology

<ul>
  {% for post in site.categories["synbio"] %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>