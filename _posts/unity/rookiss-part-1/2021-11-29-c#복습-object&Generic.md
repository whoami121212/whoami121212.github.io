---
title:  "C# - object & Generic"

categories:
  -   "Rookiss Part 1"
tags:
  - [c#, object, Generic]

toc: true
toc_sticky: true

date: 2021-11-29
last_modified_at: 2021-11-29
---

```c#
object obj = 3;
int num = (int)obj;

var var1 = 4;
int num2 = var1;
```
- object라는 타입을 이용해서 아무거나 넣을 수 있다. 하지만 특정 자료형에 할당하기 위해서는 캐스팅이 필요함. 
- **<u>c#에서는 다른 자료형들이 object를 상속받아서 만들어져있기 때문에</u>** 위와 같은 형태로 이용이 가능하다. 
    - 반면 var는 시작부터 타입이 정해져서 들어가기 때문에 따로 캐스팅이 필요하지 않다. 

```c#
// 클래스는 이렇게 만든다. 
class MyList<T> 
{
	T[] arr = new T[10]
	
	public T GetItem(int i)
	{
		return arr[i];
	}
}

// 함수는 이렇게 만든다. 
static void Test<T>(T input)
{
	// some code with T input
}

// 사용은 이렇게!
static void Main(string[] args)
{
	MyList<int> myIntList = new MyList<int>();
	Test<double>(19450.2913d);
}	
```
<br>
> 여러 클래스를 선언하지 않아도, T로만 퉁쳐서 모든 자료형을 다 이용할 수 있음. 