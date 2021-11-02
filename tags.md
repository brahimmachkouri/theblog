---
title: Liste des tags
---

{%- include search-lunr.html -%} 

<div id="lunrsearchresults">
    <ul></ul>
</div>

{% for tag in site.tags %}
  "{{ tag[0] }}" <a href="#" onClick="lunr_search('{{ tag[0] }}');">({{ tag[1].size }})</a> <br>
{% endfor %}
