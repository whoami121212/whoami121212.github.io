---
layout: single
title:  "PlayerController와 마우스 입력"
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

Tips 

- BP 내에서 마우스 오른쪽 클릭에서 검색할 때, Context Sensitive 를 꺼주면 현재 상황이나 블루프린트와 관계없는 모든 요소를 검색할 수 있다. 
  - 예: BP_PlayerController 안에서는 BP_Player가 가지고 있는 Jump() 함수를 꺼내올 수 없는데, 이 옵션을 끄면 어쨌든 가져와서 갖다놓을 수 있음. (Target을 BP_Player로 캐스팅된 Player Pawn Object가 있어야 함)

회전을 마우스 기반으로 변경 

Project Setting > Input 쪽만 변경해주고 기존과 동일하게 코드를 짠 경우, 위를 바라볼 때 캐릭터 모델의 rotation 값도 같이 변한다. 

근데 rate와 delta seconds는 곱해주지 않고 그대로 꽂는다고 함. value가 -1과 1사이를 초과하기 때문이라고 하는데? 
<img src="/../images/2024-04-29-PlayerController와 마우스입력/image-20240429091256513.png" alt="image-20240429091256513" style="zoom:67%;" />

<img src="/../images/2024-04-29-PlayerController와 마우스입력/image-20240429091317178.png" alt="image-20240429091317178" style="zoom:67%;" />

해결방법 : 

- 블루프린트 Detail의 pawn옵션 (Use Controller Rotation Pitch)를 false로 설정하고, Spring Arm의 **Use Pawn Control Rotation** 을 설정해준다.
- 중요한 것은 BP의 Detail에도, Spring Arm에도 회전을 제어하는 옵션이 있다는 것. 

이제 지금까지 작성한 코드를 모두 새로 생성한 PlayerController에 넣어준다. Target만 Get Player Pawn으로 받아오면 됨. 

