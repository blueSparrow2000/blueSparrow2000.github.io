---
title: "Misc"
layout: archive
permalink: /misc
sidebar:
    nav: "sidebar-category"
---


{% assign posts = site.categories.misc %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
