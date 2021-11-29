---
title:  "C# - abstract & Interface & Property"

categories:
  -   "Rookiss Part 1"
tags:
  - [c#, abstract, Interface, Property]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---

### abstract class 추상클래스

```c#
abstract class Monster
    {
        public abstract void Shout();
    }
    
    class Orc : Monster
    {
        public override void Shout()
        {
            Console.WriteLine("록타르 오가르!");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // 추상클래스는 생성할 수 없음 
            //Monster monster = new Monster();
        }
    }
```
#### abstract를 이용하면?
	- Monster 클래스를 상속받으면 무조건 '(override) Shout()'를 구현해야 함
	- Shout도 abstract 이기 때문에 개념적으로만 존재하고 구현되지 않은 상태이다. 
	- 더 이상 Monster로는 생성할 수 없다.

- 다중상속 불가
```c#
abstract class Flyable { public abstract void Fly(); }
		// 다중상속 안됩니다... 
    class FlyableOrc : Orc, Flyable { }
```

### Interface 인터페이스
```c#
interface IFlyable
    {
        void Fly();
    }

    class FlyableOrc : Orc, IFlyable
    {
        public void Fly()
        {
            Console.WriteLine("what the fuck...?");
        }
    }
```

#### Interface를 이용하면?
	- 반드시 구현해야 하는 것들을 정의해놓고 다중상속시켜서 강제할 수 있다. 
	- 위 다중상속 문제에서 벗어나 FlyableOrc를 구현할 수 있음.
	- 매개변수로도 인터페이스를 받을 수 있어서 유용하게 사용할 수 있다. 

### Property 프로퍼티
- 객체지향 >> 은닉성 : 불필요한 정보를 외부에 노출하지 않겠다는 뜻.
- 원래는 은닉성을 위해서 아래와 같이 처리했다. 
```c#
class Knight
{
	protected int str = 10;

	public int GetStr() { return str; } 
	public void SetStr(int str) { this.str = str; }
}
```

#### Property 활용
- 하지만~ Property를 활용하면 조금 더 쉽게 구현할 수 있다. 

```c#
class Knight
{
	protected int str = 10;
	public int Str
	{
		get { return str; }
		private set { str = value; }
	}
}

class Program
{
	static void Main(string[] args)
	{
		Knight knight = new Knight();
		int str = knight.Str;
		//knight.Str = 15;
	}
}
```

#### 자동구현 Property
- 자동구현 프로퍼티를 이용하면 **쪼-끔** 더 쉽게 구현할 수 있다. 