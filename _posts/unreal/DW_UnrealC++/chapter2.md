---
title:  "Section2 - Modern Object Oriented Programming"

categories:
  -   "DW Unreal C++"
tags:
  - [c++, unreal, oop]

toc: true
toc_sticky: true

date: 2023-10-27
last_modified_at: 2023-10-27
---

# 개요 

SOLID : 모던 객체지향 설계를 제작하기 위한 기법 
1. Single Responsibility Principle : 단일 책임 원칙, 하나의 객체는 하나의 의무만 가지도록 설계한다. 
2. Open-Closed Principle : 개방 폐쇄 원칙, 기존에 구현된 코드를 변경하지 않으면서 새로운 기능을 추가할 수 있도록 설계한다. 
3. Liskov Substitution Principle : 리스코프 치환 원칙, 자식객체를 부모객체로 변경해도 작동에 문제없을 정도로 상속을 단순히 사용한다. 
4. Interface Segregation Design : 인터페이스 분리 원칙, 객체가 구현해야 할 기능이 많다면 이들을 여러 개의 단순한 인터페이스로 분리해 설계한다. 
5. Dependency Injection Principle : 의존성 역전 원칙, 구현된 실물보다 구축해야 할 추상적 개념에 의존한다. 

## Interface 
### Interface란? 
- 객체가 반드시 구현해야 할 행동을 지정하는데 활용됨
- 다형성Polymorphism의 구현, 의존성이 분리Decouple된 설계에 유용하게 활용
- 예를 들어, 월드에 배치되는 모든 오브젝트는 Actor를 상속받고 있고, 이 중 움직이는 Actor는 Actor를 상속받은 Pawn class를 이용. Pawn들이 길찾기 기능을 반드시 구현해야 한다면 INavAgentInterface를 구현해놓으면 된다는 식. 

### Interface를 생성하면? 
- U로 시작하는 타입클래스와 I로 시작하는 인터페이스 클래스가 동시에 생성됨 
- U 타입클래스는 런타임에서 인터페이스 구현 여부를 파악하는 용도로 사용됨 
- Interface에 관련된 구성 및 구현은 I interface class에서 진행함 
- C++ Interface에서는 추상타입으로만 선언할 수 있는 C#, Java와는 다르게 인터페이스에서도 구현이 가능하다. (내부적으로는 c++ 클래스로 만들어져있기 때문에 추상화를 강제할 수 없도록 되어있음.)

## Composition 
- 복잡한 언리얼 오브젝트를 효과적으로 생성하기
- 객체지향의 설계는 크게 상속과 composition으로 설명할 수 있다. 
- 상속이 Is-A라면 컴포지션은 한 객체가 다른 객체를 소유한 Has-A관계라고 볼 수 있음 

### 목표 
- 오브젝트의 포함관계를 설계하는 방법의 습득 
- 확장 열거형 타입 사용 

### CDO가 있는 언리얼 오브젝트간의 composition을 어떻게 구현할 것인가? 
- CDO에 미리 언리얼 오브젝트를 생성해 조합한다. 
- CDO에 빈 포인터만 넣고 런타임에서 언리얼 오브젝트를 생성해 조합한다. 

일단 이걸로 하는걸로 하고, 백스페이스도 만들어놔야겠다. 귀찮아서 안되겠어. 
백스페이스는 hyper + h로 설정해서 뻑큐 날리듯이 지우는게 어떤가 하는 생각이 이렇게 삭제를 하는걸로 하고 하면 훨씬 빨리 지울 수 있스빈다. 아시겠습니까. 백스페이스를 누를 필요가 없다 습관이 되기까지는 시간이 좀 걸리겠지만 확실히 편해지겠쥬? 녹화하면서 하니까 개느리네 진짜. 이거 송출컴이 따로 있긴 해야겠다. 당황스럽네 정말. 일단 그램으로 해본다. 송출컴은 그램으로 하고, 그램이 이거를 받아야하는데 그럼 세팅이 어떻게 되지 

그램 > 캠을 받아서 송출하는데, 그램한테 던져주는 정보는 캠만 캡쳐보드가 필요없게 되는건가 마이크랑 캠만 그램에 붙어서 송출을 하게되고, 나는 공부만 하고 있으면 된다. 이 화면을 공유할 필요가 없으니까. 캡처보드가 필요한 경우는 화면을 공유할 때 뿐이니까 그럼 나느 ::::나는 이제 일단 공부하면서 