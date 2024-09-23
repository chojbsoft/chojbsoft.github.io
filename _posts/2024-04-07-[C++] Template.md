---
title: "[C++] Template"
date: 2024-04-07
author: Tigger
categories: [C++]
tags: [Template, 일반화 프로그래밍, ODR, 가변 인자, Parameter Pack, Pack Expansion, fold expression]
---

## Summary 
+ 컴파일 타임에 함수나 클래스를 생성하는 틀
+ 타입까지도 매개변수화하는 일반화 프로그래밍(제네릭 패러다임)을 지원하기 위한 문법
+ 템플릿 라이브러리는 보통 소스코드 형태로 제공됨
+ 헤더파일에 정의해도 일반 함수와는 다르게 ODR(One Definition Rule) 위반이 안생김
+ Parameter Pack과 Pack Expansion으로 가변인자 함수를 정의할 수 있음  
+ 템플릿 특수화: 해당 타입에 정확히 대응되는 정의를 발견하면 해당 정의를 사용함
+ 템플릿 메타 프로그래밍: 컴파일 타임에 연산을 끝내서, 런타임의 속도를 향상시키는 기법

## Details
프로그래밍 언어에서 바인딩이란 식별자, 연산자 등의 토큰의 속성이 구체적으로 정해지는 것을 의미합니다.
컴파일 타임에 일어나는 정적 바인딩과 런타임에 일어나는 동적 바인딩이 있습니다.
특히 가상 함수로 선언한 오버라이딩은 동적 바인딩과 관련있고, 템플릿으로 선언한 함수는 정적 바인딩 된다는 점이 중요합니다.
참고로 할당은 변수에 메모리 공간을 바인딩하는 과정으로 바인딩보다는 좀 더 좁은 개념이라고 볼 수 있습니다.

template
```cpp
template<class T1>
T add(T a, T b)
{
	retrun a + b;
}

int main()
{
	std::cout << add(3, 4) << '\n';
	std::cout << add(3.5, 4.5) << '\n';
}
```

위의 코드를 컴파일하면, 컴파일 타임 때 int 타입의 add와 double 타입의 add의 함수가 생성되어 정적 바인딩 됩니다.
아래처럼 템플릿 특수화를 통해 특정 타입에 대한 동작을 따로 정의해줄 수도 있습니다.
```cpp
template<>
double add<double>(double a, double b)
{
	return a + b - 1;
}
```

한편 이러한 템플릿 기능을 활용해서 가변 인자 함수를 구현할 수도 있습니다.
C 스타일의 가변 인자 함수는 stdarg.h 포함 및 va_list, va_start, va_arg로 구현이 생각보다 까다롭지만 C++에서는 템플릿으로 쉽게 구현할 수 있습니다.
```
void print()
{

}

template<typename... Args>
void print(Args... args) {
	// C++17의 fold expression 사용
    (std::cout << ... << args) << std::endl; 
}

int main() {
    print(10, 20, 30); // 10 20 30
    print(1, 2.5, "Hello", 'A'); // 1 2.5 Hello A
    return 0;
}
```

typename 키워드 뒤에 있는 ...은 template parameter pack이라 부르고, Args 타입 뒤에 있는 ...은 function parameter pack이라고 부릅니다.
이러한 파라미터 팩을 어떻게 다시 풀 것인가를 고민해보면 fold expression 방식과 pack expansion 방식이 있습니다.
위처럼 ... op pack의 fold expression 방식으로 구현한다면 앞에 있는 파라미터부터 계산됩니다.
pack expansion 방식으로 구현한다면 재귀 함수 형태로 동작하기 때문에, 재귀 종료 함수를 위에 정의해야 합니다.

솔직히 가변 인자 함수는 크게 사용할 일이 없어서 유용함을 잘 몰랐는데, 객체를 생성해서 특정 컨테이너나 스마트 포인터에 전달할 때는 유용할 것 같다는 생각을 최근에 했습니다.
복사 생성자나 이동 생성자를 호출하지 않는 최적화를 목적으로, 파라미터만 넘겨서 내부에서 생성자를 호출하는 환경입니다.
컨테이너나 스마트포인터의 내부에서 생성자를 호출할 때, 무슨 파라미터가 들어올지 모르기 때문입니다.

또 흥미로운 주제로 템플릿 메타 프로그래밍이라는 것이 있습니다. 
컴파일 타임에 연산을 끝내서 속도를 향상할 수 있습니다.
하지만 컴파일 타임에 연산이 되기 때문에, 그말대로 디버깅이 불가능합니다.

템플릿 메타 프로그래밍
```cpp
template<int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value; 
};

// 기본 케이스
template<>
struct Factorial<0> {
    static const int value = 1; // 0! = 1
};

int main() {
    std::cout << "5! = " << Factorial<5>::value << std::endl; 
    return 0;
}
```
