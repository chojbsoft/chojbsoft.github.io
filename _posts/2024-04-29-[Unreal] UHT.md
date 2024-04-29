---
title: "[Unreal] UHT"
date: 2024-04-29
author: Tigger
categories: [Unreal]
tags: [UHT, 리플렉션, UClass]
---

## 개념
+ 컴파일 전에 작동하는 일종의 전처리기

## 특징
+ UHT 동작 과정
	+ UCLASS, USTRUCT, UINTERFACE, UENUM, UFUNCTION, UPROPERTY 등의 매크로를 분석
	+ **.generated.h**, **.gen.cpp** 메타데이터 파일을 Intermediate 폴더에 생성
	+ 엔진이 실행될 때 해당 메타데이터를 바탕으로 각 UObject 기반 클래스에 대한 UClass 객체가 생성됨

## 장점
+ UHT의 리플렉션 시스템 덕분에 **디테일 패널에서의 조작, 블루프린트에서의 조작, 직렬화, 가비지 컬렉션, 리플리케이션** 등이 가능함

## 관련
+ 리플렉션 시스템
	+ 런타임에 객체의 속성과 기능을 조회하고 수정할 수 있는 기능
	+ 즉 런타임에 타입의 변수명 등을 알 수 있음
	+ C++ 자체에는 리플렉션 기능이 없음

## 참고
+ https://www.unrealengine.com/en-US/blog/unreal-property-system-reflection