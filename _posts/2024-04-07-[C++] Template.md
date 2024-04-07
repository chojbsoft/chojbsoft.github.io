---
title: "[C++] Template"
date: 2024-04-07
author: Tigger
categories: [C++]
tags: [Template, 일반화 프로그래밍, ODR, 가변 인자]
---

## 개념 
+ **컴파일 타임**에 함수나 클래스를 생성하는 틀

## 특징
+ 타입까지도 매개변수화하는 **일반화 프로그래밍(제네릭 패러다임)**을 지원하기 위한 문법
+ 템플릿 라이브러리는 **소스코드 형태로 제공됨**
+ **헤더파일**에 선언과 정의를 같이 해줘야함
  + 일반 함수와는 다르게 **ODR(One Definition Rule)** 위반이 안생김
+ **인자 개수가 변하는 함수**인 **가변인자 함수**를 템플릿으로 정의할 수 있음
  + C스타일 가변인자 함수와는 다르게 C++ 가변인자 템플릿은 **컴파일 타임에 타입 체크**를 수행함
  + Parameter Pack과 Pack Expansion으로 쉽게 구현 가능  
+ 템플릿 특수화
  + 해당 타입에 정확히 대응되는 정의를 발견하면 해당 정의를 사용함
+ 일부 타입들은 **타입이 아닌 템플릿인자**로 넘길 수 있음

## 장점
+ 생산성이 높아짐

## 단점
+ 컴파일 타임이 늘어남
+ lib, dll 형태로 배포가 불가능함

## 예시
+ 템플릿 함수

```cpp
template<class T1, class T2>
T1 foo(T1 a, T2 b)
{
	
}
```

+ 템플릿 특수화

```cpp
template<>
double bar(double a, double b)
{

}
```
+ c스타일 가변인자

```cpp
#include <cstdarg>
void foo(int count, ...)
{
	va_list v;
    va_start(v, count);
    for(int i=0; i<count; i++)
    {
    	int num = va_arg(v, int);
        std << num << '\n';
    }

}
```

+ c++ 가변인자 템플릿

```cpp
template<typename ... T>
void foo(T ... args)
{
	
}

```

## 관련
+ ODR
  + 선언은 여러개 있어도 되지만, 링킹에러를 방지하기 위해 정의는 하나만 있어야 한다는 규칙
+ Parameter Pack
  + 가변인자 템플릿으로 전달받은 매개변수 집합
+ Pack Expansion
  + 가변인자 함수 내에서 parameter pack을 간결하게 처리하는 것
