---
title: "의사결정트리"
layout: archive
permalink: /decisiontree/
author_profile: false
sidebar:
  nav: "docs"
---
<!-- 카테고리가 동일분류로 된것 만큼 루프 -->
{% assign posts = site.categories.decisiontree %}
  {% for post in posts %}
    {% include custom-archive-single.html type=entries_layout %}
  {% endfor %}
  
