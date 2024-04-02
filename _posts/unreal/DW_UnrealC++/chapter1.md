---
title:  "Section1 - Unreal Object"

categories:
  -   "DW Unreal C++"
tags:
  - [c++, unreal]

toc: true
toc_sticky: true

date: 2023-08-03
last_modified_at: 2023-08-03
---

# 개요 
: 

- 시작하면 editor config부터 만지고, MyGameInstance > UnrealObject 상속받은 연습용 객체 > 시작 맵 세팅 바꿔주기
- 언리얼 특 : 일단 뭐가 잘 안되면 다 끄고 vs프로젝트 파일을 열어서 다시 빌드함... 
- 한글이 제대로 출력되지 않을 때 : .cpp파일을 다른 이름으로 저장한 다음 저장 옵션에서 인코딩을 바꿔주면 된다. (UTF-8 사용)
  - 이거 자동으로 UTF-8로 해주도록 디폴트를 변경할 수 없나? 참고 : https://www.inflearn.com/questions/884213/utf-8-%ED%98%95%EC%8B%9D-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0-%EC%97%B0%EA%B5%AC-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0
- 라이브코딩 열기는 ctrl alt f11
- 검증코드 : 제대로 작동하는지 체크를 해야하는 상황들이 많은데, check와 ensure를 사용하면 좋다. 
  - check(expr) : 조건을 만족하지 않으면 에디터가 종료되고 리포트에 나온다. 
  - ensure(expr) : 조건을 만족하지 않으면 콘솔창에 빨갛게 표시가 된다. 상세 내용은 check와 동일함. 
  - ensureMsgf(expr, TEXT("smth")) : 메시지를 남길 수 있음. 
- 상위클래스 정보같은게 제대로 동작하지 않을 때에는 헤더파일 한 줄 띄우고 빌드를 한 번 해주면, generated.h 파일이 새로 생성되면서 받아올 수 있게 됩니다.
- cpp file에서는 해당 cpp file에 해당하는 header(.h) file이 가장 상단에 위치해야 한다. 그렇지 않으면 compile error 발생
- 작성한 코드에서 이름 바꾸고 싶을때는 ctrl r + ctr r 두번 누르면 된다. preview도 가능. 
- 가끔 "newLine in constant" 에러가 발생할 때가 있는데, 인코딩 문제를 해결하면 같이 해결됨. 프로젝트 생성 직후에 바로 utf-8로 만들어두는 습관을 들여야겠다. 
- 언리얼 오브젝트 헤더에서 Unreal Object Pointer를 선언할 때에는 TObjectPtr<class UStudent> 이런 식으로 감싸주어야 한다. 그리고 UPROPERTY() 붙여주어야 한다. 
- 일반 C++ 클래스는 접두사 F를 붙여준다. 
- 대충 uproject 파일만 만들어놔도 에디터가 실행된다. fileversion 이랑 editor version만 json형태로 만들어주면 됨. 

문자열 관리 체계 : FName, FText
FName : 애셋관리를 위해 사용되는 문자열 체계 
대소문자 구분 없음 Ignore Case
한 번 선언되면 바꿀 수 없음 
가볍고 빠름 
문자를 표현하는 용도가 아닌 애셋 키를 지정하는 용도로 사용. 빌드시 해시값으로 변환됨 
Global pool에 저장이 되며, FName에는 키값이 저장된다. 
FName을 Tick같이 매 프레임 실행되는 함수에 넣으면 overhead 발생할 수 있음. const static FName StaticOnlyOnce(TEXT("Goat")); 와 같은 형태로 선언해서 사용하는 것이 좋음. 
FORCEINLINE 매크로 : function overhead를 줄이기 위해서 매크로가 지정된 함수는 컴파일 시에 함수의 내용이 복사붙여넣기 된다. 

## Unreal Object 
추가로 언리얼cpp를 배워야 된다는 것이 문제 
CDO Class Default Object 
UObject를 사용하면, 모던 언어에서 사용되는 가비지콜렉터, 리플렉션 등의 기능을 사용할 수 있다. 

## Object Reflection System
- 언리얼 오브젝트의 특징과 리플렉션 시스템의 설명 

1. 언리얼 오브젝트의 특징 
- Property System이라고도 부름. 
- 프로그램이 실행시간에 자기 자신을 조사하는 기능. 
- 리플렉션 시스템에 보이도록 했으면 하는 유형이나 프로퍼티에 주석을 달아주면, Unreal Header Tool이 프록젝트를 컴파일할 때 해당 정보를 수집한다. 
> : UHT가 만들어준 ***.generated.h를 include하면, 여러가지 매크로를 사용할 수 있게 된다. UENUM(), UCLASS(), USTRUCT(), UFUNCTION(), UPROPERTY() 등의 매크로를 객체 앞에 달아주면 언리얼에서 제공하는 기능들이 따라붙는 형태. 
- 변수를 선언할 때 매크로를 붙이지 않고 운영할 수도 있지만, 메모리 등에 대해서 언리얼 에디터와는 별도로 관리를 해주어야한다. 
- 다양한 기능을 제공하기 때문에, 객체를 생성할 때 new()가 아니라 NewObject() 라는 API를 사용해야 한다. 

### Person을 상속받는 Student와 Teacher의 구현 
- 상속을 받기위해서 Person.h 파일을 include 구문의 가장 밑에 입력하면, ***.generated.h 밑에는 include 할 수 없다는 에러가 발생한다. generated가 가장 밑에 위치해야 함. 
- Reflection을 이용하면
  - 언리얼 오브젝트의 특정 속성과 함수를 이름으로 검색할 수 있다. 
  - 접근지시자와 무관하게 값을 설정할 수 있다. 
  - 오브젝트의 함수를 호출할 수 있다. 

  


