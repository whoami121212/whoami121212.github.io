---
title:  "매니저-Singleton-Managers구현"

categories:
  -   "Rookiss Part 3"
tags:
  - [Game Engine, Unity, singleton, Managers]

toc: true
toc_sticky: true

date: 2021-11-26
last_modified_at: 2021-11-26
---

### Managers class 구현
```c#
    static Managers s_Instance; // 유일성이 보장됨 
    
    // property 형식
    static public Managers Instance 
    { 
        get 
        { 
            Init(); 
            return s_Instance; 
        } 
    } // 유일하게 만들어놓은 매니저를 가지고 옴.

		static void Init()
    {
        // 아래 과정을 통해서 단 하나의 @managers의 managers 컴포넌트만 Instance에 부여된다.
        // & 호출되었을 때 null이면 새로 생성해서 할당해준다. 
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

#### 설명
- 프로그램에 하나의 Manager Instance만 생성되도록 하는 것. 
- 동작순서
    1. manager instance 호출
    2. get() method 실행
    3. init()을 통해서 'managers GameObject 생성' or 'Managers GameObject를 할당'
    4. Managers.s_Instance에 Managers component 할당
    5. return s_Instance, 즉 Managers 오브젝트의 Managers 컴포넌트의 Instance를 전달함. 