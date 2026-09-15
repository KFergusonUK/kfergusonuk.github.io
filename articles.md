---
layout: page
title: Articles
permalink: /articles/
description: "Writing by Kevin Ferguson on street works data, national systems, and AI alignment and automation."
---

<!-- This page lists every post in _posts/ automatically, newest first.
     You don't need to edit it when you publish something new. -->

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
<small>{{ post.date | date: "%B %Y" }}</small>

{{ post.excerpt }}

{% endfor %}
