---
layout: default
title: USACO Problems
permalink: /usaco/
---

USACO problems are very challenging, even for me! While my son was learning
to program, he followed along with the USACO online training website and worked
on the problems. In order to help him whenever he got stuck, I decided to also 
follow along and work through the problems... often spending WAY TOO MANY hours
to complete a single solution.

The following is a collection of my solutions. In most cases, the solutions
are my initial attempts which may not be the most sophisticated but are 
hopefully readable and easy to digest. I wrote them as an instructional aid 
for my son, so my primary concern was to make the solutions as straightforward 
as possible. In some cases, after reading through the analysis section which 
is presented on the USACO training website after the completion of each 
problem, I have gone back and revised my solutions to incorporate more 
advanced approaches to help reinforce his learning on specific techniques
such as dynamic programming.

{% assign sorted_pages = site.usaco | sort:"order" %}
{% for post in sorted_pages %}
  * [{{ post.title }}]({{ post.url }})
{% endfor %}

