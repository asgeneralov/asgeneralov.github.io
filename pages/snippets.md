---
title: Фрагменты кода
layout: default
---
# {{page.title}}
<ol class="page-list">
{% assign ordered_pages = site.snippets | sort:'order'  %}
{% for page in ordered_pages  %}
  <!-- <h2>{{ maven_member.title }}</h2> -->
  <li> 
    <a href="{{ page.url | relative_url }}">
      {{ page.title }}
    </a>
  </li>
  <!-- <p>{{ maven_member.content | markdownify }}</p> -->
{% endfor %}
</ol>