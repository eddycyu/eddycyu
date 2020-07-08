---
layout: default
title: Blog Posts
permalink: /blog/
---

The following blog posts contain short and concise explanations of various 
programming problems and concepts. They serve as a knowledge base and can be
used as a quick reference for the experienced developer or as a high-level 
introduction for the novice developer.

The most important section of each blog post is the coding example. Ideally, the
code alone can explain the concept or solution presented in the post. The code, 
primarily written in Java, is written for readability and clarity to help
promote visualization and quick understanding of the underlying concepts and 
solutions. All unnecessary boilerplate and possible optimizations, which 
would only serve to clutter and make the code more difficult to understand,
are omitted.

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}