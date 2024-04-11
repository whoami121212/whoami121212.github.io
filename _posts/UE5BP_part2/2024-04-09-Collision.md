---
layout: single
title:  "피격판정"
categories: UE5BP-2D
typora-root-url: ../
toc: true
toc_sticky: true 
toc_label: 목차
author_profile: false
sidebar:
    nav: "docs"

---

🌝 **<u>공지 사항</u>** 
이 포스트는 인프런의 [입문자를 위한 UE5-Part2. 언리얼 엔진 2D 게임 개발 입문](https://www.inflearn.com/course/%EC%96%B8%EB%A6%AC%EC%96%BC5-%EA%B0%9C%EB%B0%9C%EC%9D%98%EC%A0%95%EC%84%9D-2/dashboard) 강의를 통해 학습한 내용의 요약입니다. 자세한 내용은 해당 강의를 참고해주세요.
{: .notice--primary} 

### Tips
- BP에서 Data를 꺼내 쓸 때, 컬럼이 너무 많아서 보기 좋지 않다면 break를 사용하면 보기좋다. 
  <img src="/../images/2024-04-09-Collision/image-20240410074452530.png" alt="image-20240410074452530" style="zoom:50%;" />

-   공격 기능을 구현할 때, 피해를 받는 쪽에서 처리하는 것이 일반적이다. 

# Collision

### 개요 
공격을 했을 때, 앞에 있는 대상을 공격하는 로직을 구성한다. 구현하는 주요 기능은 아래와 같음. 
1. Creature 에서 Sprite에 Collision 세팅. 
2. BP 내에서 초기값을 설정했던 Hp와 Damage 등을 데이터에 추가해서 관리. 
3. Process Attack() 구현
	- Collision과 겹치는 객체를 찾아서, 각각의 객체에 대해서 반복문 실행 
	- 반복문 내에서는 찾은 대상에 대해서 On Damaged 함수가 실행되도록 함. 
4. 함수:OnDamaged() 구현
	-  대상이 나일 때 실행하지 않음. 
	-  Hp가 0 이하일 때에는 0으로 Set()

### 1. Collision 세팅 
Components에서 Add button을 눌러서 Box Collider를 추가한다. Sprite 밑에 붙여놔야 회전도 같이 된다. ![image-20240410084514083](/../images/2024-04-09-Collision/image-20240410084514083.png)

apply damage라는 함수도 있음 

clamp : min max 사이에서만 값이 지정된다. 
