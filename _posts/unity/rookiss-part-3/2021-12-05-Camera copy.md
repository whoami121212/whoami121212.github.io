---
title:  "카메라 (Camera)"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, Camera]

toc: true
toc_sticky: true

date: 2021-12-05
last_modified_at: 2022-10-26
---

## 쿼터뷰를 만들어보자! 

### 목적 
> Main Camera가 플레이어로부터 일정거리 떨어진채로 따라온다. 

### 개발 순서 

1. 쿼터뷰를 정의한다. 
- 당장 다른 시점이 있지도 않지만, 일단 enum으로 쿼터뷰를 정의해놓음. 
```C#
public enum CameraMode
{
    QuarterView,
}
```

2. CameraController 스크립트 생성 후 변수 초기화 
- 델타는 플레이어와 카메라의 적정거리이며, 플레이어는 태그로 찾아서 object 할당. 
```C#
    [SerializeField]
    Define.CameraMode _mode = Define.CameraMode.QuarterView;
    [SerializeField]
    Vector3 _delta;
    [SerializeField]
    GameObject _player;

    private void Start()
    {
        _delta = new Vector3(0, 6, -7);
        _player = GameObject.FindGameObjectWithTag("Player");
    }
```

3. 기본적으로는 Player의 transform.position에서 _delta만큼 떨어진 거리에 카메라를 위치시키고 Player를 바라보게 하면 됨. 
```C#
    transform.position = _player.transform.position + _delta;
    transform.LookAt(_player.transform);
```

4. 하지만 플레이어와 카메라 사이에 장애물이 있으면 카메라가 가려지게 된다. 
이 때 플레이어의 위치에서 카메라까지 출발하는 Raycast를 이용해서 장애물을 감지한 다음, 플레이어와 카메라의 거리를 장애물 앞으로 조정해주면 오케이! 
```C#
    RaycastHit hit;
    if (Physics.Raycast(_player.transform.position, _delta, out hit, _delta.magnitude, LayerMask.GetMask("Wall")))
    {
      // 그래도 정확한 거리를 계산하면 장애물과 카메라가 딱 붙어버리니까, 정확한 거리에서 0.8을 곱해준다. 
        float dist = (hit.point - _player.transform.position).magnitude * 0.8f;
        transform.position = _player.transform.position + _delta.normalized * dist;
    }
```
