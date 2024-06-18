---
layout: single
title:  ""
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

# Monster AI 

만들어야 할 블루프린트 리스트 
- BP_Monster : pawn 상속. 몬스터의 외형과 기본적인 행동 
- BP_MonsterController : AIController 상속.
- BT_Monster : Behavior Tree 상속. 
- BB_Monster : BT와 짝. Black board 상속. 

blueprint에서 설정키를 누른 다음 show inherited variables를 클릭하면 부모 클래스에게서 상속받은 변수들을 볼 수 있다. 

Behavior Tree
우클릭하면 Tasks와 Composites가 있다. Tasks는 최종적으로 행동해야하는 결과라고 할 수 있고, Composites는 분기 등을 뜻한다. 
New Task를 통해서 새로운 태스크를 생성할 수 있으며, 생성된 Task는 Receive Execute AI라는 이벤트를 통해서 실행된다. 
	- task는 성공여부가 중요하기 때문에, 반드시 Finish Execute를 통해서 성공했는지 실패했는지 알려주어야 한다. 

새로 생성한 Task : BTTask_FindPatrolLocation 
순찰할 수 있는 위치를 찾고자 한다. 이를 위해서 어디가 갈 수 있는 곳인지 파악해야한다. 
월드에 NavMeshBoundsVolume을 추가한다. P를 누르면 초록색으로 갈 수 있는 위치가 자동으로 계산된다. 
이제 준비가 되었으니, Get Random Location In Navigable Radious를 이용한다. 이름 그대로 갈 수 있는 범위 안에서 무작위의 위치를 얻어오는 함수.

Q. 위치를 찾았다고 해도 찾은 위치를 어떻게 BT로 보내줄 것인가? 
A. Blackboard를 이용하는 것이다. 
	1. 이를 위해서는 BT에서 블랙보드로 이동 후 키값을 추가해주어야한다. 
	2. BTTask에 변수를 추가해주는데, Blackboard Key Selector 타입으로 생성해야 한다. 
	3. 이후 눈모양을 클릭해서 public으로 변경해주면, BT내의 BTTask 노드에서 변수를 선택할 수 있게 된다. 

이제 BTTask 안에서 Set Blackboard Value as Vector를 통해서 값을 전달할 수 있게 되었다. 

BT : Service 
주기적으로 해야하는 일들(플레이어를 탐색해서 찾아야 한다거나)
Event Receive Tick AI 를 통해서 호출이 가능하다. 이름 그대로 매 틱당 호출되는 듯 하지만 Class Default에 가서 주기를 설정할 수 있다. 
- 서비스를 생성했다면 BT의 composites에 추가해서 사용할 수 있다. 
	이제 서비스에서 타겟을 찾는 로직을 추가해보자 
	- 일단 object type을 찾아서 넘겨야 하므로, BT->BB에서 오브젝트 타입을 추가해준다. 
	- Service_FindTarget의 로직은 아래와 같다. 
	- Get Actor of Class를 이용해서 BP_Player를 찾은 후, Controlled Pawn의 위치와 비교해서 떨어진 거리를 구한다. 떨어진 거리가 일정 이하일 때, Set Blackboard Value as Object를 이용해서 찾은 BP_Player 오브젝트를 할당한다. 이 값을 넘겨주게 되므로, 해당 서비스를 실행하면 플레이어의 위치를 찾아서 전달해주게 됨. 

BT : Decorator 
Composites에 추가할 수 있으며, Service가 실행되는 개념이라면, Decorator는 if문의 조건을 설정하는 개념이다. TargetEnemy is Is Not Set 으로 설정해두면, 적을 찾지 못했을 때에만 (랜덤위치 순찰이) 실행되도록 만들 수 있다. 
- 따라서 데코레이터 추가 후, 적을 찾은 경우에 TargetEnemy 쪽으로 이동하도록 구현하면 오케이. 

Custom Decorator 생성 
target과 range 변수를 public으로 생성해준다. 
함수는 PerformConditionCheckAI를 override 해서 사용해준다. 가장 흔한 함수. 

