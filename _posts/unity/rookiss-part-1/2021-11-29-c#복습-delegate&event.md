---
title:  "C# - delegate & event"

categories:
  -   "Rookiss Part 1"
tags:
  - [c#, delegate, event]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---

### 먼저, 이런 상황을 가정해본다.
> 순차적으로 진행되는 일이 아니라, **일을 해달라고 요청이 들어오는 경우**

#### 예) UI 작업을 할 때.
버튼을 누르면 작동하는 경우, 아래와 같은 코드를 만들었다고 하자. 
```c#
static void ButtonPressed()
{
    // codes
}
```
- 이렇게 하면...
    - UI에 관련된 코드와 게임로직이 섞여있게 된다.
    - 현실적인 문제 : 나중에 수정하기가 어렵다. 
    - 그래서 함수를 인자로 넘겨주는 방식을 생각하게 된다. (필요할 때 역으로 호출하는 방식)
```c#
static void ButtonPressed( /* 함수를 인자로 넘김 */)
{
    // 함수 호출()
}
```

### Delegate 활용
```c#
delegate int OnClicked();
        /*
         delegate형식이며, 함수 자체를 인자로 넘겨주는 형식. 
        반환은 int, 입력은 없음
        이름은 OnClicked
   */
static void ButtonPressed(OnClicked clickedFunction)
{
    clickedFunction();
}

static int DelegateTest()
{
    Console.WriteLine("delegate on!");
    return 0;
}

static void Main(string[] args)
{
    ButtonPressed(DelegateTest);
}
```
- 함수자체를 인자로 넘겨주게 된다. 
- 다양한 규칙으로 동작하는 하나의 함수를 만들 때 delegate 형식이 유용할 수 있다. 
- main 함수 작동부분은 내부적으로는 아래와 같이 동작한다. 
```c#
// OnClicked 형식으로 새로운 객체 생성(DelegateTest함수를 들고있음)
OnClicked smth = new OnClicked(DelegateTest);
// ButtonPressed의 매개변수로 전달
ButtonPressed(smth);
```

#### Delegate는 Chaining이 가능하다 : 호출되는 함수를 추가할 수 있음. 
```c#
static int DelegateTest2()
{
    Console.WriteLine("plus : delegate on!");
    return 0;
}

static void Main(string[] args)
{
    OnClicked smth = new OnClicked(DelegateTest);

    smth += DelegateTest2;

    ButtonPressed(smth);

                //ButtonPressed에서만 이용하려고 했는데, 이런 식으로 마음대로 호출이 가능함. 
                smth();
}
```
- 아무나 호출할 수 있기 때문에, 설계적인 부분에서는 문제가 있을 수 있다. 
- 보완을 위해서 delegate를 wrapping하는 문법이 있다. : event

### Event 이벤트 
```c#
// InputManager.cs 참고
class InputManager
{
// Observer Pattern
    public delegate void OnInputKey();
    public event OnInputKey InputKey;
    // 이 InputKey를 구독해놓으면, call 시 다같이 실행되는것.

    public void Update()
    {
        if (Console.KeyAvailable == false)
            return;
        ConsoleKeyInfo info = Console.ReadKey();
        if (info.Key == ConsoleKey.A)
        {
            // 모두에게 알려준다! : call back 방식으로 구현해야 함. 
            InputKey();
        }
    }
}
```
- delegate형식을 event 형식으로 한 번 더 싸놓는 것.
- InputKey에 마우스 올려보면 그냥 OnInputKey 타입으로 나옴.
- **Observer Pattern :** event를 통해서 구독자를 모집한 다음, 특정 이벤트가 발생했을 때 구독자들을 실행하는 방식
```c#
// Main
static void OnInputTest()
{
    Console.WriteLine("Input received!");
}

static void Main(string[] args)
{
    InputManager input = new InputManager();
    // 구독신청
    input.InputKey += OnInputTest;

    while(true)
    {
        input.Update();
    }
}
```
- InputKey에도 static 함수 더해놓고 call되도록 설정할 수 있다.
- **다만, delegate와는 다르게 아래의 경우처럼 마음대로 호출할 수 없다.**
- 구독 신청이나 구독 취소만 가능하다.
```c#
//input.InputKey();
```