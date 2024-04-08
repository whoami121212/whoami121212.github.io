---
layout: single
title:  "BP에서의 상속과 데이터관리"
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

## 현재 버전에서 상속이 필요한 이유
- 캐릭터(Knight) bp의 기본적인 로직을 완성한 후, 몬스터를 추가해서 개발을 계속해나가고자 한다. 
- 캐릭터와 마찬가지로 몬스터에도 로직이나 애니메이션은 동일한 코드가 포함되기 때문에, 
- 공통된 부분들만 BP_Creature로 구현하고자 한다. 



### Monster를 상속받는 Skeleton을 만들고 싶은 경우
- 몬스터에서 우클릭 후 Create Child Blueprint Class 를 선택
- event begin 함수같이, 부모 클래스에 구현되어 있는 기능을 앞서 사용하고 싶으면 "add call to parent function"을 이용하면 된다. 이 녀석은 우클릭해서 검색해도 안나오니까 주의. 

<img src="/../images/2024-04-06-inheritance&Data/image-20240408090414711.png" alt="image-20240408090414711" style="zoom:50%;" />


## 데이터 연동 

### 해결하고 싶은 문제
- Knight, Monster-Skeleton 등의 부모 클래스인 Creature에서 애니메이션 함수가 작동하고 있다. 
- Direction과 State( idle, move, skill )에 따라서 Flipbook을 재생하고 있는데, 클래스마다 적합한 Flipbook으로 변경하는 기능이 구현되지 않은 상태. 
- 따라서 Data에 Knight와 Skeleton의 Flipbook들을 구분하여 각각 적용되도록 한다. 

### 해결과정 
1. Blueprint >> Data Structure 생성 
  데이터 구조체 생성 후 데이터가 가지고 있는 속성들 정의 : Direction * State를 정의해놓는다. 
  <img src="/../images/2024-04-06-inheritance&Data/image-20240408095422018.png" alt="image-20240408095422018" style="zoom:67%;" />
2. Miscellaneous >> Data Table 생성
  구조체를 활용한 테이블을 구성한다. Knight와 Skeleton을 각각 하나의 row로 구성하면 자동으로 속성값이 따라온다.
  <img src="/../image-20240408095643988.png" alt="image-20240408095643988" style="zoom:67%;" />
3. 각 자식 클래스에서 해당하는 데이터를 Get 할 수 있도록 설정한다. 