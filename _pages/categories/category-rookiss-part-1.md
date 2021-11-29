---
title: "루키스 파트1 - C#"
layout: archive
permalink: categories/rookiss-part-1
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories['a b c'] 이런식으로! -->

***

**인프런의 'rookiss 강의 파트1 - C#' 요약**
{: .notice--warning}

{% assign posts = site.categories['Rookiss Part 1'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}