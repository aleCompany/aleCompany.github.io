---
title: "파이썬 기초"
layout: archive
permalink: /basicPy/
author_profile: false
paginate: 5 # amount of posts to show
paginate_path: /page:num/
sidebar:
  nav: "docs"
---
<!-- 카테고리가 동일분류로 된것 만큼 루프 -->
{% assign posts = site.categories.basicPy %}
  {% for post in posts %}
    {% include custom-archive-single.html type=entries_layout %}
  {% endfor %}
  
