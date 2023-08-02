# 아ㅏ아
## 아아
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
- 이를 위해서 Key / Mouse Action을 선언하고, OnUpdate 함수에서 Invoke를 실행한다. InputManager는 MonoBehaviour를 상속하고 있지 않으므로, Managers의 Update문에서 _input.OnUpdate를 실행해주어야 한다. 
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
## Resource Manager
위 InputManager와 마찬가지로, MonoBaviour를 상속받지 않고 Managers에 sigleton객체로만 선언된다. 어차피 똑같으니까 지금 중요한건 그딴게 아니고, 
- Load와 Instantiate 함수를 사용하기 편하게 재구성한다. 
### Resources.Load() wrapping
```c#
public T Load<T>(string path) where T : Object
{ 
    return Resources.Load<T>(path); 
}
```
- 아무거나 받던 Load 함수에, Object 타입만 받을 수 있도록 수정함 
### Object.Instantiate() wrapping
```c#
public GameObject Instantiate(string path, Transform parent = null)
{
    GameObject prefab = Load<GameObject>($"Prefabs/{path}");
    if (prefab == null)
    {
        Debug.Log("$Failed to load prefab : {path}");
        return null;
    }

    GameObject go = Object.Instantiate(prefab, parent);
    int index = go.name.IndexOf("(Clone)");
    if (index > 0)
        go.name = go.name.Substring(0, index);

    return go;
}
```
- prefab을 받아서 GO를 생성할 수 있도록 하는데, 경로를 다 입력할 필요없이 프리팹 폴더에 있는 이름만 넣으면 실행되도록 한다. 
- 만일 prefab을 찾는데 실패했다면 실패Log 출력
- 복사하면 생겨나는 (Clone)을 찾아서 index 번호 붙여서 수정함 


# Controller
## PlayerController
먼저 Player Control에 필요한 변수를 선언한다. 
```c#
[SerializeField]
private float _speed = 10.0f;
[SerializeField]
private float _rotationSpeed = 0.1f;
Vector3 _destPos;

public enum PlayerState
{
    Die,
    Moving,
    Idle,
}

public PlayerState _state = PlayerState.Idle;


```
- SerializeField : private이지만 에디터에서 수정할 수 있다. 

- Player의 상태를 정의해놓고 각 상태에 따라서 switch문을 실행해서 각 상태의 Update 함수(UpdateDie, UpdateMoving, UpdateIdle)를 실행한다.
    ```c#
    void Update()
    {
        switch (_state)
        {
            case PlayerState.Die:
                UpdateDie();
                break;
            case PlayerState.Moving:
                UpdateMoving();
                break;
            case PlayerState.Idle:
                UpdateIdle();
                break;

        }
    }
    ```

- 각 상태에 대한 Update함수들은 아래와 같다.(아직 Die랑 Idle은 별거없다)
    ```c#
        void UpdateDie()
    {

    }

        void UpdateIdle()
    {
        Animator anim = gameObject.GetComponent<Animator>();
        anim.SetFloat("speed", 0);
    }

    void UpdateMoving()
    {
        Vector3 dir = _destPos - transform.position;
        if (dir.magnitude < 0.0001f)
        {
            _state = PlayerState.Idle;
        }
        else
        {

            float moveDist = Mathf.Clamp(_speed * Time.deltaTime, 0, dir.magnitude);
            transform.position += dir.normalized * moveDist;

            transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(dir), 20 * Time.deltaTime);
               
        }
        Animator anim = gameObject.GetComponent<Animator>();
        anim.SetFloat("speed", _speed);
    }
    ```
    - UpdateMoving()을 한 줄씩 해석하자면
    1. 가야할 방향인 dir 변수는 목표지점과 현재 플레이어의 위치를 뺀 값이다. Vector3로서, 방향과 크기를 모두 가지고 있다. 
    2. dir의 크기가 0.0001보다 크면 idle 상태로 변경한다. Idle 상태가 되면 Update()에서 UpdateIdle()이 실행되어 정지 애니메이션이 실행된다. 
    3. else : 한 프레임당 가야할 거리를 계산한다. 
        - moveDist는 Mathf.Clamp를 이용해서 계산한다. 
        - _speed와 deltaTime을 곱한 값 == 지금 속도로 1프레임에 이동하는 거리 
        - 1프레임에 이동하는 거리는 dir.magnitude값을 넘을 수 없다. 즉, 목표지점에 도착하면 지나치는게 아니라 멈추어야 한다. 
    4. rotation값은 dir 방향을 보도록 천천히 회전하게 Quaternion.Slerp 함수를 이용한다. 
    5. 애니메이션 재생 : _speed 변수에 따라서 재생되도록 Animator 조정
    
<br>

> PlayerController에서는 마우스클릭을 이벤트로 받아서 실행하는 OnMouseClicked() 함수가 가장 중요하다. Start()함수에서 MouseAction에 함수를 등록해두고, 나중에 마우스가 클릭될때마다 이벤트를 받아서 실행되도록 한다. 
    ```c#
    void Start()
    {
        Managers.Input.MouseAction -= OnMouseClicked;
        Managers.Input.MouseAction += OnMouseClicked;
    }

    void OnMouseClicked(Define.MouseEvent evt)
    {
        if (_state == PlayerState.Die)
            return;
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        Debug.DrawRay(Camera.main.transform.position, ray.direction * 100.0f, Color.red, 1.0f);

        RaycastHit hit;
        if (Physics.Raycast(ray, out hit, 100.0f, LayerMask.GetMask("Wall")))
        {
            _destPos = hit.point;
            _state = PlayerState.Moving;
        }
    }
    ```

<br>

### 실행순서
    1. Managers - _input.OnUpdate() 가 계속 실행되고 있음. 
    2. _input객체(InputManager)가 OnUpdate()함수를 통해서 매 프레임마다 마우스 입력을 확인 중
    3. 마우스 입력이 확인되면 Invoke를 통해서 추가해두었던 마우스이벤트를 실행시킨다. 


