---
layout: page
title: About
description: 打码改变世界
keywords: FEHub
comments: true
menu: 关于
permalink: /about/
---

> 世界太辽阔，将你的消息淹没。

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
{% if site.url contains 'fehub.net' %}
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ assets_base_url }}/assets/images/qrcode.jpg" alt="FE世界" />
</li>
{% endif %}
</ul>


## 友情链接

<ul>
{% for link in site.data.links %}
{% if link.src == "life" %}
<li><a href="{{ link.url }}" target="_blank">{{ link.name }}</a></li>
{% endif %}
{% endfor %}
</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
