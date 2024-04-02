# 은닉성
접근지정자 : public protected private
protected : 나를 상속받아서 생성된 클래스는 사용할 수 있음. 
현실적 예시 : 누가 나한테 피해를 주었는지 추적해서 기여도 처리해야 하는 상황. 누적 데미지를 변수로 만들었을 때, 이 변수의 경우에는 외부에서 다른 요인으로 건드리면 안되는 수치이다. 그렇기 때문에 private로 설정하는 것이 옳다. 

# 다형성 
예시 : Actor -> Monster -> Orc 로 상속되는 클래스들이 있고, Monster에 Shout()을 생성, Orc는 Shout()을 override 하는 상황이라고 가정함. 
결국 function override를 말하는 것인데, 블루프린트에서는 어떻게 하는가? 
Function 옆에 override 드롭다운 메뉴가 있다. 클릭하면 override할 수 있는 함수들 목록이 나옴. Parent 노드를 끊고 새로 작성해주면 된다. 물~론 parent를 그대로 호출한 다음에 추가로 코드를 작성할 수 있다. 

C타입들은 parent class에서 함수를 만들 때 처음부터 virtual이라는 키워드를 추가해두어야 override 할 수 있다. 

만약 SpawnActor BP Orc로 Orc 객체를 생성한 다음, typeof Monster로 변수를 만들어서 Set을 한 후 Shout을 호출하면? 
만들어진 객체가 Orc로 생성되었지만, Monster로 Set 되어있는 상태. 이 상황에서 호출되는 Shout은 부모클래스의 함수인가? 
맞지않을까??
**처음 생성했던대로 Orc->Shout()이 실행된다.**

처음 만들 때에만 어떤 타입인지가 중요하다. 기능도 문제가 없기 때문에 monster 타입으로 모아서 관리할 수 있다는 뜻. 



# Q. 
블루프린트에서 생성한 함수, 변수랑 C++ 코드가 모두 호환 되는가? 



