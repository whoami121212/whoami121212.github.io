# Managers
## 싱글톤
- static으로 정의한 다음, field와 property를 동시에 선언한다.
- s_Instance를 직접 get할 수 없으며, property인 Instance를 통해서만 가능하다. 
- Instance를 get하면 Managers.Init()을 실행한 후 s_Instance를 return한다. 
```c#
static Managers s_Instance
static Managers Instance{ get {Init(); return s_Instance}}
```

- Start()함수에서 Managers.Init()을 실행
- s_Instance란, Managers component를 말하는데 이게 없다는 뜻은 현재 Managers component가 붙어있는 GameObject가 없거나, 있더라도 실행되지 않았다는 뜻. 
```c#
- DontDestroyOnLoad : 새로운 Scene을 불러오는 중에도 삭제되면 안되기 때문. 항상 남아서 역할을 해야하는 경우에 실행
static void Init()
{
    // 매니저의 인스턴스가 없는 경우, @Managers를 찾고, 없으면 만들어서 component 붙여주기
    if (s_Instance == null)
    {
        GameObject go = GameObject.Find("@Managers");
        if (go == null)
        {
            go = new GameObject { name = "@Managers" };
            go.AddComponent<Managers>();
        }
        DontDestroyOnLoad(go);
        s_Instance = go.GetComponent<Managers>();
    }
}
```

## InputManager
- Managers 안에 InputManager를 싱글톤으로 선언해둔다. 이를 통해서 안에 정의된 내용들은 InputManager가 아니라 Managers.Input을 이용해서만 접근하게 된다.(갖다 쓸 수 있는 유일한 객체)
- Managers에 정의된 InputManager는 아래와 같다. 
```c#
// Managers.cs
InputManager _input = new InputManager();
public static InputManager Input { get { return Instance._input; } }
```

- InputManager는 Delegate를 이용하는데, Action을 등록해두었다가 필요할 때 Invoke()를 실행시키는 형태이다. 
- 이를 위해서 Key / Mouse Action을 선언하고, OnUpdate 함수에서 Invoke를 실행한다. InputManager는 Monobehavior를 상속하고 있지 않으므로, Managers의 Update문에서 _input.OnUpdate를 실행해주어야 한다. 
- 변수의 선언과 OnUpdate함수는 아래와 같다.
```c#
public Action KeyAction = null;
public Action<Define.MouseEvent> MouseAction = null;
bool _pressed = false;

public void OnUpdate()
{
    if (EventSystem.current.IsPointerOverGameObject())
        return;
    if (Input.anyKey && KeyAction != null)
        KeyAction.Invoke()
    
    if (MouseAction != null)
    {
        if (Input.GetMouseButton(0))
        {
            MouseAction.Invoke(Define.MouseEvent.Press);
            _pressed = true;
        }
        else
        {
            if (_pressed)
            {
                MouseAction.Invoke(Define.MouseEvent.Click);
            }
            _pressed = false;
        }
    }
}
```



# Controller
## PlayerController


