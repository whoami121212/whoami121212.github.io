---
layout: single
title:  "애니메이션"
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

## Tips

details > Character Movement(Rotation Settings) > orient rotation to movement : 이동하는 방향으로 회전

## 애니메이션 

- 애니메이션 블루프린트 생성
- 상태머신 설정 
  - 애니메이션 블루프린트는 개미굴처럼 파서 들어가는 형태로 만들어져 있다. 
  - State machine 하나를 설정해서 idle, move start, move, move end로 이어지는 상태를 설정한다. 
    <img src="/../images/2024-04-30-Animation/image-20240430070555277.png" alt="image-20240430070555277" style="zoom: 80%;" />

- 이벤트그래프에서 조건 설정 
  - 현재 폰의 속도를 구해서 0이상이면 bMoving을 true로 설정해준다. 
  - State machine에서 Moving이 true일 때 Move Start를 거쳐서 Move로 이동하도록, false일 때 MoveEnd를 거쳐서 Idle 상태로 가도록 수정. 
  - 한 번 설정한 rule은 share해서 다른 노드의 연결에 사용할 수 있다. (위 사진에 색깔이 동일한 조건으로 공유된 것들)

## 애니메이션 몽타주 

- 이동하면서 총을 쏘는 애니메이션을 만들어야 함. 하지만 현재 스테이트머신에서 총쏘는 애니메이션을 추가하기가 어려움. 이럴 때 애니메이션 몽타주를 사용할 수 있음. 

Group-Slot-Section으로 이루어져 있음. 

anim slot manager를 통해서 새로운 슬롯을 만들어 줄 수 있음. 일단 몽타주만 만들어둔 상태 

PlayerController에서 마우스 왼쪽 클릭을 하면 발사하도록 만들어놓고, Player에서 Fire 함수를 만들어 Play Anim Montage를 호출한다. 하지만 현재 시점에서는 아무것도 작동하지 않는다. 

ABP_Player에서 몽타주를 세팅해주어야 함. 

Child Montage도 만들 수 있음. 
