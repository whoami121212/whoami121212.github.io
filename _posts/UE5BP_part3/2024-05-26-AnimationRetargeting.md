---
layout: single
title:  "애니메이션 리타겟팅"
categories: UE5BP-3D
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"

---

🌝 **<u>공지 사항</u>** 
이 포스트는 인프런의 [입문자를 위한 UE5-Part3. 언리얼 엔진 3D 게임 개발 입문](https://www.inflearn.com/course/%EC%96%B8%EB%A6%AC%EC%96%BC5-%EA%B0%9C%EB%B0%9C%EC%9D%98%EC%A0%95%EC%84%9D-3/) 강의를 통해 학습한 내용의 요약입니다. 자세한 내용은 해당 강의를 참고해주세요.
{: .notice--primary} 

# 

없는 애니메이션을 가져온다면 어떻게 될까? 

마켓플레이스에서 가져온다면... 뼈대를 움직이고 있는것이 애니메이션 정보 

큰 규모를 만든다면 많은 플레이어들이 있을텐데. 2족보행을 하는 캐릭터들이 많을텐데, 공통적인 부분들이 있을 것이기 때문에 변형을 시켜서 움직일 수 있다면 이상적인 상황일 것. 

리타겟팅이라는 것은 다른 뼈대에 맞게 적용시키는 것이라고 생각하면 된다. 
