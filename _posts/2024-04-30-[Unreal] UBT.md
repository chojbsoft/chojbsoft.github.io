---
title: "[Unreal] UBT"
date: 2024-04-30
author: Tigger
categories: [Unreal]
tags: [UBT, Build.cs, Target.cs]
---

## 개념
+ 크로스 플랫폼 빌드를 지원하기위한 일종의 전처리기

## 특징
+ Generate Visual Studio project files로 인해 실행됨
+ 개발 환경 플랫폼에 맞게 **솔루션과 프로젝트 파일들을 생성**함
+ 이렇게 UBT가 프로젝트들을 계속 재생성하기 때문에, **Build.cs와 Target.cs**에서 프로젝트 설정을 해야함
+ Build.cs
	+ **각 모듈마다의** 외부 모듈 추가, 추가 포함 디렉토리 설정 등의 작업
+ target.cs
	+ **전체 프로젝트의** 빌드 방식, 유니티 빌드 설정, PCH 설정 등의 작업

## 예시
+ Build.cs

```cs
// UMG 모듈을 사용하기 위해서 추가
PublicDependencyModuleNames.AddRange(new string[]
{ 
	"Core", "CoreUObject", "Engine", "InputCore", "UMG"
});

// 추가 포함 디렉토리
PublicIncludePaths.Add("Module");
PublicIncludePaths.Add("Module/Actors");
```

## 관련
+ 컴파일 타임의 프로그램이기 때문에, 속도보다 편의성에 유리한 닷넷으로 개발됨

## 참고
+ <https://ericlemes.com/2018/11/23/understanding-unreal-build-tool/>
