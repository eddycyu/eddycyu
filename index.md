---
layout: default
title: "Making the world a better place, one code snippet at a time."
---

### Latest Posts

{% for post in site.posts offset:0 limit:4 %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
    {{ post.excerpt }}
{% endfor %}
