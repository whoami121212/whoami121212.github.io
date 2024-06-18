---
layout: single
title:  "에임 오프셋 Aim Offset"
categories: UE5BP-3D
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"

---

🌝 **<u>공지 사항</u>** 
이 포스트는 인프런의 [입문자를 위한 UE5-Part3. 언리얼 엔진 3D 게임 개발 입문](https://www.inflearn.com/course/%EC%96%B8%EB%A6%AC%EC%96%BC5-%EA%B0%9C%EB%B0%9C%EC%9D%98%EC%A0%95%EC%84%9D-3) 강의를 통해 학습한 내용의 요약입니다. 자세한 내용은 해당 강의를 참고해주세요.
{: .notice--primary} 

기능에서 ctrl alt를 누르면 자세한 설명이 나온다. 

에임오프셋에 대한 내용입니다. 
Aim offset은 blend space와 비슷하지만 덧붙일 수 있다는 개념이다. additive 

Blend 두 애니메이션을 섞는데 
Apply Mesh Space Additive 이거는 추가하는 기능이다. 따라서 잘못 추가하면 기괴해지는데, 애니메이션 자체를 '추가용'으로 변경해두면 이런 문제가 사라진다. 
Additive Settings > Additive Anim Type 을 조정해서 기반이 되는 애니메이션에 덧붙이는 개념으로 만들어주면 된다. 


TPS 느낌을 내기 위해서, 현재 세팅에서 카메라가 약간 오른쪽에서 캐릭터를 찍는 느낌을 주고 싶다면? 
Camera의 Socket Offset을 설정하면 된다. 
당연히 기능이 있을 것이라고 생각할 것! 

