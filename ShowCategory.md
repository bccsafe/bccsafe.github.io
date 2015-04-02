---
layout: page
title: 文章归档
---
 
{% for category in site.categories %}
{% if category[0] contains 'wp_' %}
{% else %}
<h2>{{ category | first }}({{ category | last | size }})</h2> 
<ul>
    {% for post in category.last %}
        <li>{{ post.date | date:"%Y/%m/%d"}}	<a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
{% endif %}
{% endfor %}