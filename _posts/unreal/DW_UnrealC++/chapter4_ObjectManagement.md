## 개괄 
- JSON은 JavaScript Object Notation 의 약자이다. 
    - 텍스트임에도 데이터가 가볍고, 이해하기 쉬움. 웹 통신의 표준으로 사용됨. 
    - 지원하는 타입이 한정적. 다시 말해서 그냥 숫자는 하나 뿐. 
    - {} 오브젝트, [] 배열

# 언리얼엔진이 제공하는 직렬화의 이해
직렬화하고 이를 저장하고 불러들이는 방법을 알아본다... 하지만 직렬화가 무엇인가? 

디스크나 네트워크로 전송할 수 있는 일렬로 구성된 바이트스트림으로 변경 
바이트스트림을 복구하는 과정도 포함한다. 

장점 
- 저장하고 복원할 수 있음 : 게임의 저장 등 
- 객체의 정보를 다른 프로그램으로 전송 
- 네트워크로 전달해서 복원 : 네트워크 게임 
- 압축/암호화를 통해서 안전하게 보관가능

회전 같은 정보들을 모두 보낼 필요는 없기 때문에 데이터를 줄여서 보낼 수 있는데 이것을 양자화라고도 한다. 
올바로 해석해서 원상복구 시키는 것도 기술적으로 고려해야 할 부분. 
에러처리 등도 설계해야하기 때문에 쉬운 일이 아님. 

언리얼에서는 FArchive를 통해서 가능함.

# 패키지 
- 복잡한 계층구조를 가진 언리얼 오브젝트를 효과적으로 저장하고 불러들이는 방법 
- 이를 위해서 UPackage단위로 언리얼 오브젝트를 관리한다. 
- 언리얼 오브젝트를 감싼 포장 오브젝트를 의미한다. 

- Game Packaging : 개발된 최종 콘텐츠를 정리해 프로그램으로 만드는 작업. 
- DLC와 같이 향후 확장 콘텐츠에 사용되는 별도의 데이터 묶음을 의미하기도 한다. (pkg파일)

- Unreal Object Package의 Subobject를 Asset이라고 한다. 에디터에는 이 Asset이 노출된다. 
    - Package는 다수의 Unreal Object를 소유할 수 있으나, 일반적으로는 하나의 Asset만 가진다. 
    - Asset은 다수의 Subobject를 가질 수 있으며, 모두 언리얼 오브젝트 패키지에 포함된다. 하지만 에디터에는 노출되지 않는다. 


# 에셋 참조와 로딩 방법
에셋간 연결할때마다 동적으로 메모리를 할당하는 것은 비효율적 
에셋의 로딩 전략 
- 프로젝트에서 에셋이 반드시 필요한 경우 : 생성자 코드에서 미리 로딩 
- 런타임에서 필요한 때 로딩하는 경우 : 런타임 코드에서 정적 로딩 
- 런타임에서 비동기적으로 로딩하는 경우 : 런티임 로직에서 관리자를 사용해 비동기 로딩 

## Object Path 오브젝트 경로 
- 패키지 이름과 애셋 이름을 묶은 문자열 
- 에셋 클래스 정보는 생략할 수 있음
- 패키지 내 데이터를 모두 로드하지 않고 오브젝트 경로를 사용해 필요한 에셋만 로드할 수 있음 
    - {에셋클래스정보}'{패키지이름}.{에셋이름}' 
    - 혹은 {패키지이름}.{에셋이름}

- 이미 생성 또는 로드된 UObject만 사용하는 경우에는 FindObject<>()
- 로드되어 있지 않은 오브젝트를 로드하려면 LoadObject<>()가 맞음. 내부적으로는 FindObject와 동일한 역할을 하므로, 검색한다음 로드할 필요가 없다. 

## Asset Streamable Manager 
- 에셋의 비동기 로딩을 지원하는 관리자 객체 
- 콘텐츠 제작과 무관한 싱글턴 클래스에 FStreamableManager를 선언해두면 좋음. (GameInstance는 좋은 선택지 )
- 다수의 오브젝트 경로를 입력해 다수의 에셋을 로딩하는 것도 가능함. 

# 언리얼 빌드 시스템 
## 에디터의 특징 
- 개발 작업을 위해서 다양한 폴더와 파일 이름 규칙이 미리 설정되어 있다. 
- 정해진 규칙을 잘 파악하고 프로젝트 폴더와 파일을 설정해야 함. 

 