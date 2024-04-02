---
title:  "레이캐스팅 (Raycasting)"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, Raycasting]

toc: true
toc_sticky: true

date: 2021-11-30
last_modified_at: 2021-12-06
---
> 오늘의 목표 : 
내가 보고 있는 화면에서 3D 영역을 클릭하는 방법 = 카메라에서 클릭한 방향으로 레이저를 쏘아서 맞는 곳을 return

# Raycasting - 1

## Raycasting 레이캐스팅 기초 
-  현재 position에서 ray를 쏘아서 마주치는 오브젝트의 정보를 불러오는 코드

```c#
void Update()
{
  // 로컬 좌표인 transform.position이 현재 바라보는 "앞"쪽의 정보를 월드좌표로 변경해서 look에 저장
  Vector3 look = transform.TransformDirection(Vector3.forward);

  // 실행 시 ray를 볼 수 있도록 그려준다. (transform.position에서 위쪽으로 조금 올린 곳에서 시작, 로컬좌표의 앞방향인 look의 10배, 빨간색 레이)
  Debug.DrawRay(transform.position + Vector3.up, look * 10, Color.red);

  // ray와 부딪힌 정보를 저장할 hit을 선언 
  RaycastHit hit;
  // raycast가 true일때 = 오브젝트와 부딪혔을때 (현재위치에서, look방향으로, 정보는 hit에 저장하고, 10만큼 쏜당~)
  if (Physics.Raycast(transform.position, look, out hit, 10))
  {
    // hit정보 중에 collider에 해당하는 gameObject의 이름을 Log로 찍음
     Debug.Log($"Raycast {hit.collider.gameObject.name}!");
  }
}
```
<br>

### RaycastAll 
- 일반 Raycast는 처음 부딪힌 오브젝트만 반환하는데 걸리는 놈들은 다 때려박아서 배열로 반환할 수도 있음. 
  
```c#
  RaycastHit[] hits;
  hits = Physics.RaycastAll(transform.position, look, 10);

  foreach (RaycastHit hit in hits)
  {
      Debug.Log($"RaycastAll {hit.collider.gameObject.name}!");
  }
```
<br>

## 좌표의 변환
- Local <-> World <-> (Viewport <-> Screen(화면) : 거의 비슷) 
<br>

### screen 좌표 
: 스크린 상의 픽셀좌표가 표시된다. 
```c#
Debug.Log(Input.mousePosition);
```

![image](https://user-images.githubusercontent.com/53845159/144255301-1924a6be-413b-4b1b-a0d7-32e5a4c2197e.png)

<br>

### screen -> viewport 좌표
: x & y 좌표가 0~1 사이 값이 나온다. (비율로 표시됨)
```c#
Debug.Log(Camera.main.ScreenToViewportPoint(Input.mousePosition));
```
![image](https://user-images.githubusercontent.com/53845159/144255478-87d31b20-99b5-40ea-9764-beaf95c0cddd.png)


<br>

### screen -> world 좌표로
: 현재 마우스 위치의 월드좌표
- 참고: camera => clipping planes 카메라로 찍히는 범위를 정함

![image](https://user-images.githubusercontent.com/53845159/144255550-a6d1b543-50bf-4822-8cf0-266405151c0c.png)

```c#
// 마우스 버튼을 누르면? 
if (Input.GetMouseButtonDown(0))
{
    // 지금 play화면을 그리고 있는 화면의 mousePos는 = 카메라의 스크린포인트를 월드포인트로 변환(마우스x, 마우스y, 카메라에서 화면까지 떨어진 거리)
    Vector3 mousePos = Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.nearClipPlane));
    // normalize(dest - start) = 방향벡터
    Vector3 dir = mousePos - Camera.main.transform.position;
    dir = dir.normalized;

    Debug.DrawRay(Camera.main.transform.position, dir * 100.0f, Color.red, 1.0f);

    RaycastHit hit;
    // 이제 카메라 위치에서 마우스위치로 ray를 쏘아서 닿는 지점을 return하면 그게 바로 클릭한 월드 포인트
    if (Physics.Raycast(Camera.main.transform.position, dir, out hit, 100.0f))
    {
        Debug.Log($"Raycast camera hit : {hit.collider.gameObject.name}");
    }
}
```

#### 개쉬운버전이 있음 - Ray 객체 이용 
```c#
if (Input.GetMouseButtonDown(0))
{
    // 이 Ray 객체를 이용해서 Physics.Raycast를 다 이용할 수 있다. 
    Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);

    Debug.DrawRay(Camera.main.transform.position, ray.direction * 100.0f, Color.red, 1.0f);

    RaycastHit hit;
    if (Physics.Raycast(ray, out hit, 100.0f))
    {
        Debug.Log($"Raycast camera hit : {hit.collider.gameObject.name}");
    }
}
```

- 테스트

![image](https://user-images.githubusercontent.com/53845159/144255705-31774ad2-9ca2-45bc-994b-b98274dc637e.png)



# Raycasting - 2

## Layer Mask

### Layer Mask 활용

Raycast의 버전 중 아래 버전을 이용하면 layerMask를 지정할 수 있다. 
```c#
bool Physics.Raycast(Ray ray, out RaycastHit hitinfo, float maxDistance, int layerMask )
```
 - layerMask는 총 32개로, int32를 이용하고 있다고 할 수 있다. 
 - 아래 유니티에서 설정해준대로 해당 비트를 체크한다고 보면 된다. 

 ![image](https://user-images.githubusercontent.com/53845159/144754033-b746f407-f1e6-4bfc-a4ec-a2a270d6660c.png)


#### LayerMask 실습 

1. 만약에 8번째 비트를 켜고싶다면 :
```c#
int mask = (1 << 8);
```

2. 8번과 9번 : 몬스터와 벽을 모두 체크하고싶다면 or연산을 활용 : 
```c#
int mask = (1 << 8) | (1 << 9);
```

3. 만약에 위 방법이 너무 어렵다면? 가독성있게 만들수도 있다.
- 성능상 차이는 크지 않다고 한다... 
```c#
LayerMask mask = LayerMask.GetMask("Monster") | LayerMask.GetMask("Wall");
```
