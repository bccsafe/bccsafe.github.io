---
layout: page
title: 原Blog文章归档</br>(2012.10 - 2015.2)
---

{% for category in site.categories %}
{% if category[0] contains 'wp_' %}
<h2>{{ category | first }}({{ category | last | size }})</h2> 
<ul>
    {% for post in category.last %}     
        <li>{{ post.date | date:"%Y/%m/%d"}}	<a href="{{ post.url }}">{{ post.title }}</a></li>    
    {% endfor %}
</ul>
{% endif %}
{% endfor %}
