---
title: Apache Maven
layout: default
---
# Apache Maven
<ol class="page-list">
{% assign ordered_pages = site.maven | sort:'order'  %}
{% for maven_member in ordered_pages  %}
  <!-- <h2>{{ maven_member.title }}</h2> -->
  <li> 
  <a href="{{ maven_member.url | relative_url }}">
    {{ maven_member.title }}
  </a>
  </li>
  <!-- <p>{{ maven_member.content | markdownify }}</p> -->
{% endfor %}
</ol>