---
title:  "충돌 (Collision)"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, collision, trigger, physics]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---

### 기억해야 할 내용들
- mass의 기준은 kg
- use gravity
- is kinematic
- constraints

<br><br>

#### collision & trigger test code : 
```c#
private void OnCollisionEnter(Collision collision)
{
    Debug.Log($"collision on : {collision.gameObject.name}");
}

private void OnTriggerEnter(Collider other)
{
    Debug.Log($"trigger on : {other.gameObject.name}");
}
```
<br>

### Collision Matrix
Collision detection occurs and messages are sent upon collision

![image](https://user-images.githubusercontent.com/53845159/143886956-bb32e412-3e33-46c6-97da-55f01f692b61.png)

1. 나 혹은 상대에게 RigidBody가 있어야 한다 + Is Kinematic : off
2. 나한테 Collider가 있어야 한다 + Is Trigger : off
3. 상대에게 Collider가 있어야 한다 + Is Trigger : off

Trigger messages are sent upon collision

![image](https://user-images.githubusercontent.com/53845159/143887237-79b27e01-28a4-43a6-9eb1-562719ad5278.png)

1. 둘 다 collider가 있어야 한다. 
2. 둘 중 하나는 Is Trigger : On
3. 둘 중 하나는 RigidBody가 있어야 한다.