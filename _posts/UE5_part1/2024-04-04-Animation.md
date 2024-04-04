---
layout: single
title:  "애니메이션 갱신"
categories: UE5BP-2D
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"

---

🌝 **<u>공지 사항</u>** 이 포스트는 인프런의 [입문자를 위한 UE5-Part2. 언리얼 엔진 2D 게임 개발 입문](https://www.inflearn.com/course/%EC%96%B8%EB%A6%AC%EC%96%BC5-%EA%B0%9C%EB%B0%9C%EC%9D%98%EC%A0%95%EC%84%9D-2/dashboard) 강의를 통해 학습한 내용의 요약입니다. 자세한 내용은 해당 강의를 참고해주세요.
{: .notice--primary} 

- 에디터 내에서 플레이 후 F8을 누르면 빠져나와서 BP에서 어디가 실행되고 있는지 확인할 수 있다. 

## 기존 로직 
- axis 설정 후 방향키를 누르면 그 쪽을 바라보는 애니메이션이 재생
### 수정이 필요한 부분 
- key를 누르지 않았을 때에는 방향에 따라 idle 
- 누르고 있을 때에는 move 애니메이션이 재생되어야 함 
### 수정사항들
- "버튼을 누르지 않은 상태 == axis value가 0인 상태" 에서도 한 방향을 바라보고 있어야 하므로, enum type을 생성해서 바라보는 Direction을 저장하도록 한다. 

<img src="/../images/2024-04-04-Animation/image-20240404222138271.png" alt="image-20240404222138271" style="zoom: 50%;" />

- BP 안에 변수로 선언하고 입력된 axis value에 따라서 set()을 실행한다. 

<img src="/../images/2024-04-04-Animation/image-20240404222313754.png" alt="image-20240404222313754" style="zoom:50%;" />

- "버튼이 눌린상태 == 이동, 누르지 않은 상태 == 정지" 이므로, 매 tick마다 axis value를 확인해서 boolean Keyboard Pressed를 체크한다. 

<img src="/../images/2024-04-04-Animation/image-20240404222554292.png" alt="image-20240404222554292" style="zoom:50%;" />

- 애니메이션의 변경에 대한 내용은 Event Graph에 몰아넣지 말고, 따로 Update Animation 함수로 빼서 관리한다. 
  현재 KeyboardPressed와 Direction을 저장해두었기 때문에 각 상황에 맞는 애니메이션을 재생하도록 설정. 

<img src="/../images/2024-04-04-Animation/image-20240404222704383.png" alt="image-20240404222704383" style="zoom:50%;" />



