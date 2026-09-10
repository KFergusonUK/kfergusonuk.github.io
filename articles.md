---
layout: page
title: Articles
permalink: /articles/
---

<!-- This page lists every post in _posts/ automatically. You don't
     need to edit it when you publish something new.

     NOT yet in the nav bar. Once you have three or four posts here,
     add "- articles.md" to header_pages in _config.yml. -->

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
<small>{{ post.date | date: "%B %Y" }}</small>

{{ post.excerpt }}

{% endfor %}
