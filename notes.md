---
layout: default
title: Developer Notes
permalink: /notes/
---

The following notes contain short and concise recipes to various 
programming questions. They serve as a knowledge base and can be used as a
quick reference by the experienced developer and as a high-level  
introduction by the novice developer.

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
    {{ post.excerpt }}
{% endfor %}