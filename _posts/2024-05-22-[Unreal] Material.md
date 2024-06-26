---
title: "[Unreal] Material"
date: 2024-05-22
author: Tigger
categories: [Unreal]
tags: [Material, Material Instance, Dynamic Material Instance, HLSL, usf, PBR, 환경맵, mipmap]
---

## 개념 
+ 오브젝트의 색, 질감 등을 표현하는 함수

## 특징
+ 머티리얼 생성
	+ 머티리얼을 생성하면 **HLSL 문법**으로 변환이되어, **Saved 폴더 내의 usf 셰이더 파일**에 삽입이되고 컴파일됨
		+ 언리얼 에디터를 로드할 때 셰이더 컴파일 중인 것이 이 과정
		+ **custom 노드**를 생성해서 직접 HLSL 코드를 넣을 수 있음
+ 머티리얼 인스턴스
	+ 만들어놓은 머티리얼을 기반으로, 컴파일 없이 값만 수정해서 사용할 수 있음
	+ 여러 오브젝트가 기본 머티리얼 구조는 같지만, 색상이나 텍스쳐가 다른 경우 사용함
+ 다이나믹 머티리얼 인스턴스
	+ 런타임에 속성 변경 가능한 머티리얼
+ 블렌드 모드 설정
	+ 설정에 따라 머티리얼 결과 노드가 바뀜
	+ Opaque는 불투명
	+ Masked는 색칠할지말지
	+ Translucent는 투명이라 오파시티로 알파값 설정 가능
	+ Additive는 위에 덧그리는 형태
+ PBR의 핵심요소
	+ 베이스컬러, 메탈릭, 러프니스
+ 속성
	+ Base Color
		+ **조명이 없을 때의 기본적인 색상**
		+ 베이스컬러가 0~1값의 비율인 이유는 RGB8보다 더 많은 비트를 사용하는 포맷도 있기 때문
		+ 벡터 파라미터 노드의 R, G, B 채널은 해당 채널의 값으로 통일되어 벡터가 반환됨
	+ Metalic
		+ 높을수록 금속적으로 되어 반사 정도가 높아짐
		+ **주변 환경맵(큐브맵)의 혼합 비율**로 표현됨
	+ Roughness
		+ 낮을수록 부드러워져 반사 정도가 높아짐
		+ 환경맵의 해상도로 표현됨
		+ 낮을수록 해상도가 낮은 환경맵의 **mipmap**으로 교체되는 것
	+ Specular
		+ 비금속이라고 가정했을 때의 반사 정도

## 관련
+ 실시간 렌더러는 오프라인 렌더러와 다르게, 실제로 반사시키는 것이 아니라 액터 주변을 캡처한 큐브맵을 사용해서 반사를 표현함
+ 환경맵
	+ 오브젝트 주변 환경을 캡처한 텍스처
+ mipmap
	+ 원본 텍스처를 기준으로 절반씩 작은 크기들을 미리 저장하는 기술
	+ 텍스쳐 에디터의 디테일 패널의 LOD섹션에서 민맵 메서드 설정 가능
	+ mipmap을 사용하지 않으면 멀리 있는 오브젝트들은 노이즈가 생기고, GPU 메모리에 여러번 텍스쳐를 올려야됨	
+ 조금 떠보일 때는 스페큘러를 0으로해서 하얀부분제거하기
	+ 스페큘러의 기본값이 0.5이기 때문
+ 오파시티보다 오파시티 마스크 컬링이 더 빠르지만 덜 자연스러움
  