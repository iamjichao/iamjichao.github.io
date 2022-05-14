---
layout: page
title: Links
description: 没有链接的博客是孤独的
keywords: 友情链接
comments: true
menu: 链接
permalink: /links/
---

> 官方文档

<ul>
{% for link in site.data.links %}
  {% if link.src == 'doc' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 学习网站

<ul>
{% for link in site.data.links %}
  {% if link.src == 'www' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 前端工具库

<ul>
{% for link in site.data.links %}
  {% if link.src == 'tool' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 友情链接

<ul>
{% for link in site.data.links %}
  {% if link.src == 'life' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>
