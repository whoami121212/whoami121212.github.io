---
layout: single
title:  "시작"
categories: UE5BP-CPP
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"


---

계속 빌드에 문제가 있으니까 ㅠ 너무 힘들다 
C++은 너무 예민하니까 프로젝트를 유지하기 위한 전략
- 첫 빌드 성공 시 깃헙에 올려두기 
- 버전관리 지속적으로 하기 
- 문제 생기면 바로 롤백하면서 변경사항 체크하기 

이제 파트4 시작합니다. 

### 파일의 생성과 삭제 

- 파일의 생성 : 
  cpp와 header 파일을 어떤 식으로 만들든 상관없이, Source 폴더 안에만 만들어지면 generate visual studio 명령을 통해서 불러오기가 가능하다. 다만 에디터에서 만드는게 자동으로 생성되니까 편한 건 있음. 
- 파일의 삭제 : 
  원본 소스를 그대로 따라가기 때문에 폴더에서 삭제하면 된다. 
- 이름의 변경 : 폴더에서 변경하고 generate 
- visual studio 안에서 삭제나 생성, 변경을 하면 귀찮다. 폴더를 만드는 행동도 아무런 의미가 없음. 
- **결론 : 삭제를 제외하고 파일을 다루는 것은 에디터에서 하는게 제일 편하다.**

### 언리얼 코딩표준 
- 문서 읽어볼 것. 대체로는 아는 것들이다. 
- TEXT 매크로는 이것저것 호환성 때문이다. 

### Modules 
- 프로젝트를 생성할 때 디폴트 모듈이 만들어지기 때문에 프로젝트당 1개의 모듈이 존재한다고 생각하면 된다. 
- 새로운 모듈을 생성할 수 있으며, 생성에 대해서는 [문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-modules-in-unreal-engine)를 확인하면 된다. 
- 모듈을 생성하고 나면 이후 새로 C++ 클래스를 만들 때 어떤 모듈에 포함될지 지정할 수 있다. 이런 식으로 분리시킬 수 있는듯. 
  ![image-20240817232152283](/../images/Untitled/image-20240817232152283.png)
- 재사용 등에 용이하기 때문에 코드와 기능을 나누고싶을 때 사용하면 된다. 

### Reflection & UObject 
- 생성자 : new가 아닌 NewObject<>() 를 이용한다. 
- reflection은 Xray를 찍는 느낌이다. 메모리 관리랑 엮여있음. 참조를 이용해서 메모리를 관리하기 때문에. 그렇기 때문에 UObject -> 를 이용해서 Object안에 구현되어있는 리플렉션 문법을 이용할 수 있다. (UObject -> AddToRoot() 등)

c++ 클래스를 만든 다음, 이것을 상속받은 블루프린트 클래스를 만들어서 관리할 수 있다. 

### Material 머티리얼 
삼총사를 알아야 한다. 
- texture : 이미지 
- mesh : 크게는 SM, SKM 로 나뉜다. 
- material 
- 동일한 머티리얼이지만, 파라미터만 바뀌거나 하는 경우에는 material instance라는 것을 만들어서 베리에이션을 할 수 있다. MI_를 사용한다. 

