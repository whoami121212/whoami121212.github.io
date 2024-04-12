---
layout: single
title:  ""
categories: UE5BP-2D
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"

---

🌝 **<u>공지 사항</u>** 
이 포스트는 인프런의 [입문자를 위한 UE5-Part2. 언리얼 엔진 2D 게임 개발 입문](https://www.inflearn.com/course/%EC%96%B8%EB%A6%AC%EC%96%BC5-%EA%B0%9C%EB%B0%9C%EC%9D%98%EC%A0%95%EC%84%9D-2/dashboard) 강의를 통해 학습한 내용의 요약입니다. 자세한 내용은 해당 강의를 참고해주세요.
{: .notice--primary} 

# 제목

유니티와의 차이점 : 

다수의 Knight 오브젝트를 맵에 배치해두었을 때, 클래스 안에 Input에 대한 함수가 구현되어 있기 때문에 유니티라면 모두 동시에 움직이게 될 것이다. 하지만 언리얼에서는 Possess라는 개념이 있고, 하나의 객체에만 Player 0번이 배치되어 있기 때문에 하나만 움직이게 된다. (Auto Possess Player 메뉴가 Player 0번으로 설정되어 있음. )



BP_Knight.UpdateInput() 안에서 모든 입력과 이동을 처리하고 있는데, 나중에 만들다보면 Input안에 온갖 액션들이 포함되게 되면서 복잡해진다. 

룰이 바뀌는 경우에도 종속성이 너무 강해진다. 

따라서 입력에 대한 부분을 별도의 클래스를 구성해서 빼놓는게 좋다는 결론에 이르게 된다. 
