---
title: Archives blog
layout: page
---

<ul class="archive-years">
<li><a href="/archive/archives" title="archives" rel="{{ cat[1].size }}">All ({{site.posts.size}})</a></li>
  {% assign counter = 0 %}
  {% for post in site.posts %}
    {% assign thisyear = post.date | date: "%Y" %}
    {% assign prevyear = post.previous.date | date: "%Y" %}
    {% assign counter = counter | plus: 1 %}
    {% if thisyear != prevyear %}
        <li><a href="/archive/{{ post.date | date:"%Y" }}">{{ thisyear }} ({{ counter }})</a></li>
      {% assign counter = 0 %}
    {% endif %}
  {% endfor %}
</ul>

<ul>
{% for post in site.posts  %}
    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

    {% if forloop.first %}
    <h2 id="{{ this_year }}-ref">{{this_year}}</h2>
    <ul>
    {% endif %}

    <li><a href="{{ post.url }}">{{ post.title }}</a></li>

    {% if forloop.last %}
    </ul>
    {% else %}
        {% if this_year != next_year %}
        </ul>
        <h2 id="{{ next_year }}-ref">{{next_year}}</h2>
        <ul>
        {% endif %}
    {% endif %}
{% endfor %}
</ul>