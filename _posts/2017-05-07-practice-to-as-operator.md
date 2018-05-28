---
layout: post
title: Practice to as operator
description: as 연산자를 예제로 이해한다.
category: programming
tags: [csharp]
comments: true
---

[.Netframework: as (C# Reference)](https://docs.microsoft.com/en-us/dotnet/articles/csharp/language-reference/keywords/as)를 바탕으로 구성되었습니다.

`as`연산자는 호환 가능한 참조 타입(reference type)끼리 형변환합니다.
만약 형변환하지 못 한다면, 할당 가능한
[nullable type](https://docs.microsoft.com/en-us/dotnet/articles/csharp/programming-guide/nullable-types/index)
을 가져옵니다. 별도로 정의하지 않는 이상 `null`입니다.

## 예제

여러 타입의 변수를 string으로 형변환해보고 그 결과에 따라 서로 다른 문자열을 출력해보겠습니다.
`objArray`에는 총 6가지 타입, `ClassA`, `ClassB`, `string`, `int`, `double`, `null` 순으로 저장됩니다.
`for` 구문에서는 순차적으로 `as`연산자를 사용하여 `string`으로 형변환한 후, 그 결과에 따라 다음과 같이 출력합니다.

- `string`이 맞을 경우: 문자열을 출력합니다.
- `string`은 아니지만 `null`이 맞을 경우: string이 아니라 null이라고 출력합니다.
- `string`도 아니며 `null`도 아닐 경우: string이 아니라 다른 타입이라고 출력합니다.

``` cs
using System;

class ClassA { }
class ClassB { }

class MainClass
{
    static void Main()
    {
        object[] objArray = new object[6];
        objArray[0] = new ClassA();
        objArray[1] = new ClassB();
        objArray[2] = "hello";
        objArray[3] = 123;
        objArray[4] = 123.4;
        objArray[5] = null;

        for (int i = 0; i < objArray.Length; ++i)
        {
            string s = objArray[i] as string;
            Console.Write("{0}:", i);
            if (s != null)
            {
                Console.WriteLine("'" + s + "'");
            }
            else
            {
                if (objArray[i] == null)
                    Console.WriteLine("not a string. -> null");
                else
                    Console.WriteLine("not a string. -> " + objArray[i].GetType().Name);            }
        }
    }
}

/*
Output:
0:not a string. -> ClassA
1:not a string. -> ClassB
2:'hello'
3:not a string. -> Int32
4:not a string. -> Double
5:not a string. -> null
*/
```

### 각 변수별 타입 설명

- `ClassA`, `ClassB`: `Reference` 타입으로 개발자가 정의한 클래스입니다. 따라서 `string`이 아닙니다. 정의한 클래스명이 그대로 출력됩니다.  
- `string`: `Reference` 타입으로 `System.string` 클래스이비다.  
- `123`: `Int32` 타입이며, 이는 `Value` 타입의 일종입다. C#에서는 int가 키워드이며, 실제로는 `System.Int32` 클래스로 할당됩니다.
- `123.4`: `double` 타입이며, 이는 `Value`타입의 일종입니다.  C#에서는 double이 키워드이며, 실제로는 `System.Double` 클래스로 할당된다. 참고로 C#에서 `float`도 키워드이며, 실제로는 `System.Single`로 할당됩니다.
- `null`: C#에서 `null`은 문자형 키워드로 '없음'을 나타냅니다.
