---
layout: page
title: About
description: 菜鸟笔记
keywords: 菜鸟笔记
comments: true
menu: 关于
permalink: /about/
---

一个菜鸟的笔记

stay hungry stay foolish

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
