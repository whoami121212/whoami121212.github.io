---
title: "기획"
layout: archive
permalink: categories/story
author_profile: true
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories['a b c'] 이런식으로! -->

***

**내맘대로 기획해보는 페이지입니다. 비밀글임**
{: .notice--warning}

{% assign posts = site.categories['Story'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}