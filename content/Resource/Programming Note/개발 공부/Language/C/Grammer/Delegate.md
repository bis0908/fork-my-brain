---
tags:
  - C
---

1. 기본 형식

```
delegate 반환형 delegate_name(매개변수..);
```

1. 메서드를 가리킬 수 있는 타입이다.
2. 메소드를 참조로, 매개변수로 넘길 수 있다.
3. 당연하지만 메서드와 동일한 매개변수, 리턴타입으로 선언 필요

```C
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
namespace ConsoleApplication39 {		
// example 1    
delegate int PDelegate(int a, int b);    
class Program    {        
static int Plus(int a, int b)        
{            return a + b;        }        
static void Main(string[] args)        
{            PDelegate pd1 = Plus;            
 PDelegate pd2 = delegate(int a, int b)            
 {                return a / b;            };            Console.WriteLine(pd1(5, 10));            
 Console.WriteLine(pd2(10, 5));        }    }}
```

```c
// example 2
delegate int MyDeligate(string s);
static void Test(){	MyDeligate m = new MyDeligate(StringToInt);	Run(m);}static int StringToInt(string s){	
return int.Parse(s);}
static void Run(MyDeligate m){	int i = m("123");	
 Write(i);}
```

  

- delegate 참고 사이트

> [!info] C# delegate (콜백, 체인, 범용성)  
> C#에서 메서드를 가리킬 수 있는 타입 (C++의 함수 포인터, 함수형프로그래밍에서 일급 함수와 유사하다.) delegate로 선언한 타입이 메서드를 가리키기 때문에 그 메서드를 직접 호출하는 것 대신에 delegate로 그 메서드를 호출할 수 있다. - 메서드를 직접호출하면되지 왜 굳이 delegate를 사용해서 호출할까? 는 잠시 후에.. 1. delegate 만들기 delegate가 어떤 메서드를 가리키기 때문에 그 메서드와 동일한 매개변수와 리턴타입으로 선언해야한다.  
> [https://jeong-pro.tistory.com/51](https://jeong-pro.tistory.com/51)  

> [!info] [C#] C#의 Delegate  
> delegate를 한 마디로 말하자면, "함수를 대입할 수 있는 변수"이다. 코드로 설명한다면 더 빨리 와닿을 것 같지만, 그 전에 Delegate형을 선언하는 방법부터 설명하고자 한다. 방금 Delegate는 함수를 대입하는 변수라고 설명했다. 이 말은 즉, 함수에는 "반환값"과 "인수"가 필요하다는 것이다. 아래와 같은 형식으로 선언한다.  
> [https://engineer-mole.tistory.com/176](https://engineer-mole.tistory.com/176)  

  

- delegate와 관련된 개념(CallBack)

> [!info] [C# 고급문법] \#2 Delegate(델리게이트) & 콜백함수(CallBack)  
> 목차] * 개인적인 공부 기록용으로 작성한 포스팅 이기에 잘못된 내용이 있을 수 있으며, 추가하거나 잘못된 내용이 있다면 지속적으로 수정해 나갈 예정입니다. 프로그래머는 함수를 호출할 때 콜(Call)을 하여 호출합니다. 예를 들어, Run() 이라는 함수를 사용하기 위해 콜(Call)을 하여, 함수를 실행해 달라고 요청을 하는 것이지요. 콜백(CallBack)은, 콜의 반대 되는 개념입니다.  
> [https://novlog.tistory.com/47](https://novlog.tistory.com/47)