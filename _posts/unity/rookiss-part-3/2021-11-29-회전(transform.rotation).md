---
title:  "회전(transform.rotation)"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, transform, 이동]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---

### 회전에 관하여...

```c#
float r_speed = 10f;
transform.eulerAngles += new Vector3(0f, r_speed *Time.deltaTime, 0f);
```
- transform의 rotation 속성을 변경한다. 
- transform.eulerAngles는 연산을 하는 방식은 좋지 않다. 
    - 연산이 필요할 때에는 아래 Rotate를 이용

```c#
transform.Rotate(new Vector3(0f, Time.deltaTime * 100.0f, 0f));
```
- 회전의 void 함수
- 하지만 transform.rotation 속성에 대한 변경은 Quaternion을 이용한다. 

```c#
transform.rotation = Quaternion.Euler(new Vector3(0f, r_speed * Time.deltaTime, 0f));
```
- rotation을 직접 건드림. 
- Quaternion에 많은 함수들이 있다. rotation을 수정하고자 할 때 이용하면 좋음.

```c#
transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(Vector3.forward), _rotationSpeed);
```
- Slerp를 이용해서 (시작점, 목표점, 속도)를 매개변수로 천천히 방향전환을 하는 기능을 구현할 수 있다. 