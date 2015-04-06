---
layout: blog
title: /dev/urandom - Random Blog Posts
permalink: /blog/urandom/
author: all
---

# urandom - Random Blog Posts 

<p> Named after /dev/urandom </p> 

{% for post in site.categories.urandom %}
  {% include blog_item.html %}
{% endfor %}
