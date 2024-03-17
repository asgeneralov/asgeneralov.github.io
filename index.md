---
layout: default
title: "Blog"
---
<h2>Циклы статей</h2>
{%- assign default_paths = site.pages | map: "path" -%}
{%- assign page_paths = site.header_pages | default: default_paths -%}
<ul class="page-list">
    {%- for path in page_paths -%}
    {%- assign my_page = site.pages | where: "path", path | first -%}
    {%- if my_page.title and my_page.url != "/" -%}
    <li><a class="page-link" href="{{ my_page.url | relative_url }}">{{ my_page.title | escape }}</a></li>
    {%- endif -%}
    {%- endfor -%}
</ul>

<h2>Разные заметки</h2>
<ul class="post-list">
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}"><span class="title">{{ post.title }} </span>&nbsp;<span class="date">({{ post.date | date: "%d.%m.%Y" }})</span></a>
    </li>
  {% endfor %}
</ul>

