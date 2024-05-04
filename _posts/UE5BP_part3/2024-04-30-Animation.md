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

## 블렌드 스페이스 Blend Space

시작 전 세팅 : Use Controller Rotation Yaw를 true, orient rotation to movement을 false로 해주어서 캐릭터가 항상 카메라의 정면을 바라보게 한다. 

- 여기부터 앞으로 가는 애니메이션과 섞어서 게걸음과 뒷걸음을 구현할 것. 
  이를 위해서는 조준하고 있는 방향과 움직이고 있는 방향의 차이의 rotation값을 이용해서 애니메이션을 섞어주어야 한다. 

- 이미 Pawn에 Get Base Pawn Aim Rotation이라는 함수를 통해서 현재 조준하는 rotation값을 가져올 수 있음. 
  <img src="/../images/2024-04-30-Animation/image-20240501174203171.png" alt="image-20240501174203171" style="zoom:80%;" />
  - Get Velocity를 이용해 현재 이동 중인 값을 구한다. 이를 Rotator, 즉 x값 normailize된 값으로 변환하여 준비. 
  - Get Base Aim Rotation 함수를 이용해서 현재 조준하고 있는(카메라가 바라보고있는) 방향의 Rotator 값을 구한다. 
  - 이 두 값을 뽑아서 Yaw만 return하면 두 rotator의 차이, 즉 -180~180도 까지 범위내의 값이 구해진다. 
- 최종 구해진 MovementOffsetYaw 값에 따라서 180이면 뒤로 걷고, 0이면 앞으로, 90도나 -90도면 옆으로 걷도록 만들면 된다. 

### Blend Space 세팅

- 블렌드 스페이스를 생성하고 이름을 BS_Move로 지어준다. 
- Asset Browser에서 에셋을 갖다놓으면 각각 변수의 위치에 따라서 적절히 애니메이션이 섞이도록 설정할 수 있다. 
- Horizontal Axis의 값을 MovementOffsetYaw로 정해두고 -180부터 180의 값을 가지게 한다. 
- 그래프에 적절한 애니메이션을 배치하고 섞어주면 끝! 
  <img src="/../images/2024-04-30-Animation/image-20240501174816965.png" alt="image-20240501174816965" style="zoom:50%;" />



## 루트 본 회전
선 상태로 Yaw를 돌렸을 때 상반신과 하반신이 즉시 같이 움직이는 것이 아니라, 상반신만 둘러보는 식으로 움직이다가 90도 이상 돌아갔을 때 다리가 같이 움직이도록 하기 위함. 

