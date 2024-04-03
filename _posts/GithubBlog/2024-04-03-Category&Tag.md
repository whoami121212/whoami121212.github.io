---
layout: single
title:  "카테고리와 태그 추가"
categories: Github Blog
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"
---

# Category & Tag 기능 추가

1. config 파일에서 카테고리 주석 제거

<img src="/../images/2024-04-03-Category&Tag/image-20240403121708867.png" alt="image-20240403121708867" style="zoom: 50%;" />



2. _pages 폴더에 category-archive.md와 tag-archive.md 파일 추가 

<img src="/../images/2024-04-03-Category&Tag/image-20240403121816677.png" alt="image-20240403121816677" style="zoom:50%;" />

3. data 폴더에 navigation.yml 파일 수정 (상단 네비게이션 바 설정인듯)

<img src="/../images/2024-04-03-Category&Tag/image-20240403121905054.png" alt="image-20240403121905054" style="zoom:50%;" />

## 사이드바에 카테고리를 추가하고 싶은 경우 
1. navigation.yml에 새로운 형태를 정의한다. 

![image-20240403223029355](/../images/2024-04-03-Category&Tag/image-20240403223029355.png)

2. 문서에서 새로 정의한 "docs"라는 이름의 메뉴를 사이드바에 붙인다. 

![image-20240403223105032](/../images/2024-04-03-Category&Tag/image-20240403223105032.png)





# 검색기능 추가
1. _pages에 search.md 파일 만들어 두기 

![image-20240403222834080](/../images/2024-04-03-Category&Tag/image-20240403222834080.png)

2. navigation.yml 파일에 카테고리와 똑같은 형태로 추가하면 끝! 



