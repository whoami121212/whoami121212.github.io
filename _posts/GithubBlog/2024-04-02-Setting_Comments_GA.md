---
layout: single
title:  "댓글기능 & 구글 애널리틱스"
categories: Github Blog
typora-root-url: ../
toc: true
---

# 댓글기능(Comments)을  추가함

Minimal Mistakes의 가이드문서에서 disqus를 설치해서 댓글을 추가함. 

disqus.com에서 할 수 있으며, 이후 config.yml 파일에서  comments 관련 기능 설정으로 사용가능. 

<img src="/../images/2024-04-02-Comments/image-20240402122205641.png" alt="image-20240402122205641" style="zoom:50%;" />

이 부분과 가장 하단의 comments를 true 체크

<img src="/../images/2024-04-02-Comments/image-20240402122258457.png" alt="image-20240402122258457" style="zoom:50%;" />



로그인 후 어드민으로 들어가면 댓글에 대한 관리나 통계정보 확인도 가능하다. 

# Google Analytics

Google Analytics 사이트에서 분석을 원하는 사이트를 추가하고, 연결이 완료되면 tracking_id를 복사한다. 
이후 config.yml 파일을 아래와 같이 수정해주면 된다. 
<img src="/../images/2024-04-02-Comments/image-20240402123543721.png" alt="image-20240402123543721" style="zoom:50%;" />
