---
title: 关于
layout: page
---

### 个人说明

* Qi Li
* IBM大数据研发工程师
* 博客逐步完善中，敬请期待 :)

### 欢迎交流

* 邮箱：[{{ site.email }}](mailto:{{ site.email }})
* 网站：[https://qili93.github.io/](https://qili93.github.io/)
* GitHub : [http://github.com/{{ site.hub }}](http://github.com/{{ site.hub }})
* 豆瓣：[https://www.douban.com/people/selma_lq/](https://www.douban.com/people/selma_lq/)

<!--ul>
{% assign counter = 0 %}
{% for post in site.posts %}
  {% assign thisyear = post.date | date: "%B %Y" %}
  {% assign prevyear = post.previous.date | date: "%B %Y" %}
  {% assign counter = counter | plus: 1 %}
  {% if thisyear != prevyear %}
    <li><a href="/archive/#{{ post.date | date:"%B %Y" }}">{{ thisyear }} ({{ counter }})</a></li>
    {% assign counter = 0 %}
  {% endif %}
{% endfor %}
</ul-->