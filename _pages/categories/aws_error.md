---
title: "AWS 관련오류"
layout: archive
permalink: /aws_error/
author_profile: false
paginate: 5 # amount of posts to show
paginate_path: /page:num/
sidebar:
  nav: "docs"
---
<!-- 카테고리가 동일분류로 된것 만큼 루프 -->
{% assign posts = site.categories.aws_error %}
  {% for post in posts %}
    {% include custom-archive-single.html type=entries_layout %}
  {% endfor %}
