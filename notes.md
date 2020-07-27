---
layout: default
title: Developer Notes
permalink: /notes/
---

The following notes contain recipes to various programming questions and 
development tasks. They have been documented to serve as a reference for
those who need to get things done quickly, and as reading material for
the curious.

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
    {{ post.excerpt }}
{% endfor %}