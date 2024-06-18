---
layout: single
title:  "충돌관련"
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

Collision component나 기본 이론에 대해서는 이미 알고있으니 넘어간다. 

## RayCasting
Line Trace By Channel
Line Trace By Profile : Profile은 Preset을 의미한다. 
Line Trace For Objects : Make Array 등으로 배열을 집어넣어서 해당하는 오브젝트와 충돌처리를 한다. 

Box Trace 버전도 있음. 

## Cross Hair
### HUD blueprint (BP_AimHUD)
1. HUD를 상속받는 BP를 만들어준다. 
2. Event Receive Draw HUD : 매 프레임마다 호출. size는 화면의 크기라고 볼 수 있음. 
3. Draw Texture : 텍스처를 배치하는데, '화면의 중간지점'을 구해서 넣어주어야 한다. 
4. 화면 사이즈의 1/2만큼 구해서 배치하면 된다. 
5. 크로스헤어의 배치가 끝났지만, 이제 발사를 눌렀을 때 화면상 크로스헤어가 가리키는 방향에 있는 object를 찾는 과정이 필요하다. 

### BP_Player.FireWepon() 안에서 Raycasting 
1. Get Viewport Size를 이용하면 현재 화면의 크기를 구할 수 있고, 1/2 때리면 중앙의 좌표를 구할 수 있다. Make Vector 2D로 2D vector로 만들어준다. 
2. Deproject Screen to World : Transforms the given 2D screen space coordinate into a 3D world-space point and direction.
3. Line Trace by Channel로 카메라 위치에서 타겟을 확인하는 부분까지 완료가 되었다면 다음으로 넘어간다. 

### 총구에서 발사되도록 
1. 총구의 위치를 정하는 소켓 생성 후, Get Socket Transform을 이용해서 불러온다. 
2. 소켓에서 발사되는 로직으로 수정하면 끗!
- 