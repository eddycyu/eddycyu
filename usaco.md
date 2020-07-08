---
layout: default
title: USACO Problems
permalink: /usaco/
---

USACO problems are very challenging, even for me! While my son was learning
to program, he followed along with the USACO online training pages and worked
on the problems. In order to help him whenever he got stock, I also followed 
along and worked through the problems... often spending WAY TOO MANY hours to
complete a single problem.

The following is a collection of my solutions. In most cases, the solutions
are my initial attempts which may not be very sophisticated but are hopefully
readable and easy to understand as I used them as a teaching tool for my son. 
In some cases, I have revised my solutions to incorporate more sophisticated 
approaches after reading through the analysis section which is presented on 
the USACO training website after the completion of each problem. For all 
cases, I have tried to always focus the solutions more towards readability 
than making the fastest code possible, while still performing fast enough to 
pass all the tests.

{% assign sorted_pages = site.usaco | sort:"order" %}
{% for post in sorted_pages %}
  * [{{ post.title }}]({{ post.url }})
{% endfor %}

