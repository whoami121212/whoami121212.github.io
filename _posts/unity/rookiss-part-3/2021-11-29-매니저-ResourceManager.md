---
title:  "매니저-Resource Manager"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, Managers]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---
## listener pattern
매니저 클래스 안에서 이벤트를 체크해서, Action으로 전파하는 방법 


### 1. 다른 GameObject를 불러오는 가장 쉬운 방법 - 코드로 명시하고 엔진에서 때려박기
```c#
// 테스트 : prefab을 선언해서 코드에 붙인 다음, Instantiate()로 GameObject에 할당한다. 		
public GameObject prefab;

GameObject dummy;

void Start()
{
    dummy = Object.Instantiate(prefab, this.transform);

    Destroy(dummy, 5.0f);
}
```
> 이렇게 하면 아래와 같이 엔진에서 오브젝트를 붙일 수 있습니다. 
<br>

![image](https://user-images.githubusercontent.com/53845159/143876163-c3ca2557-fc57-4842-b4e1-bcb879b6977c.png)

### 2. 코드로 제어
```c#
// GameObject를 private으로 선언(엔진에 노출되지 않음)
GameObject prefab;
GameObject dummy;

void Start()
{
    prefab = Resources.Load<GameObject>("Prefabs/Dummy");
    dummy = Object.Instantiate(prefab, this.transform);

    Destroy(dummy, 5.0f);
}
```
- 기본적으로 Resources.Load<T>()함수는 Assets/Resources 폴더 안에서 불러온다. ()안에는 그 경로 이후의 경로만 입력해주면 됨. 
<br>

![image](https://user-images.githubusercontent.com/53845159/143876396-74915e8e-a647-4c1b-b84c-c8ff88849508.png)

>  **하지만...** 결국 리소스 관리를 위한 <u>resource manager</u>가 필요하게 된다...

### 3. Resource Manager
```c#
public class ResourceManager
{
		// Resources.Load를 리소스매니저에서 쓸 수 있도록 wrapping
    public T Load<T>(string path) where T : Object
    { 
        return Resources.Load<T>(path); 
    }

    public GameObject Instantiate(string path, Transform parent = null)
    {
				// Prefabs 폴더 건너뛰도록 설정. 이제 폴더 안에 있는 오브젝트 이름만 넣어도 됨. 
        GameObject prefab = Load<GameObject>($"Prefabs/{path}");
        if (prefab == null)
        {
            Debug.Log("$Failed to load prefab : {path}");
            return null;
        }

        return Object.Instantiate(prefab, parent);
    }

    public void Destroy(GameObject go)
    {
        if (go == null)
            return;

        Object.Destroy(go);
    }
}
```
- MonoBehaviour 지움 : 상속받을 필요가 없기 때문에
- 이런 형태로 매니저를 구성한 다음, Managers 객체 안에서 Resource Manager를 사용하는 형태로 구현한다. 결국, singleton으로 생성된 유일한 Managers 객체에서 모든 manager들을 제어하게 되는 것. 
```c#
public class Managers : MonoBehaviour
{
// 생략 // 
    InputManager _input = new InputManager();
    ResourceManager _resource = new ResourceManager();
    public static InputManager Input { get { return Instance._input; } }
    public static ResourceManager Resource { get { return Instance._resource; } }
// 생략 // 
}
```
- 이렇게 Managers에서 온갖 매니저들의 인스턴스를 다 만들어놓는거임. 
- 그리고 사용할 때는 아래와 같이! 
  - 아래 코드를 보면, 이제 Managers의 (유일한)인스턴스를 이용해서 리소스를 불러오고 할당/삭제하는 등의 작업을 할 수 있다.
```c#
public class PrefabTest : MonoBehaviour
{
    GameObject dummy;
    void Start()
    {
        dummy = Managers.Resource.Instantiate("Dummy");

        Managers.Resource.Destroy(dummy);
    }
}
```