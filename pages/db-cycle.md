---
title: Хранение данных
layout: default
---
# {{ page.title}}
<ol class="page-list">
{% assign ordered_pages = site.db | sort:'order'  %}
{% for member in ordered_pages  %}

  <li> 
  <a href="{{ member.url | relative_url }}">
    {{ member.title }}
  </a>
  </li>
  
{% endfor %}
</ol>