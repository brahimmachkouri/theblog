---
title: Liste des tags
---

{% for tag in site.tags %}
  <h3>Tag : "{{ tag[0] }}" ({{ tag[1].size }}):</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
