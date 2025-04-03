---
title: Сопровождение
layout: default
---
# {{page.title}}
<ol class="page-list">
{% assign ordered_pages = site.devops | sort:'order'  %}
{% for devops_member in ordered_pages  %}
  <!-- <h2>{{ maven_member.title }}</h2> -->
  <li> 
  <a href="{{ devops_member.url | relative_url }}">
    {{ devops_member.title }}
  </a>
  </li>
  <!-- <p>{{ maven_member.content | markdownify }}</p> -->
{% endfor %}
</ol>