---
title:  "이동(transform.position) 관련"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, transform, 이동]

toc: true
toc_sticky: true

date: 2021-11-28
last_modified_at: 2021-11-28
---

### 로컬좌표 기준 이동

```C#
//update 안에서 실행
float _speed = 10;
if (Input.GetKey(KeyCode.W))
{
	transform.position += transform.TransformDirection(Vector3.forward * Time.deltaTime * _speed);
}
```
- W키를 누르면 앞으로 이동하는 코드.
- TransformDirection : 로컬 좌표를 월드좌표로 변환해줌. 즉, 위의 코드를 이용 시 현재 go의 로컬 좌표를 중심으로 앞으로 움직인다. 

```C#
transform.Translate(Vector3.forward * Time.deltaTime * _speed)
```
- 위의 코드와 동일하게 작동함. 

### 월드좌표 기준으로 이동
```C#
transform.positio += Vector3.back * Time.deltaTime * _speed
```
- position에 바로 벡터 방향과 크기를 곱해주면 월드좌표 기준으로 이동한다. 

참고) 회전과 합쳐서 이동을 완성한다. 