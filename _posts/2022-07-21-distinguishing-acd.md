---
title: (KR) 함수형코딩 | 03-액션과 계산, 데이터의 차이를 알기
author: mangodm
date: 2022-07-21 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 3장을 읽고, 정리한 페이지

---

# 03. 액션과 계산, 데이터의 차이를 알기

- 개발 과정 전반(설계 - 코딩 - 코드 리뷰)에 걸쳐 액션, 계산, 데이터를 구분하는 기술을 적용할 수 있음.

## 설계 Tips 
- 하나의 액션처럼 보이는 것들을 액션, 계산, 데이터로 세분화하는 연습이 필요함.
- 계산 단계는 머릿속에서 자동으로 이뤄져 찾기 어려움. 다음의 두 질문을 의식적으로 생각하면 계산을 찾는데 도움이 됨.
    - 어떤 단계에서 무엇인가 "결정"해야 할 것이 있나?
    - 무엇인가 "계획"해서 방법을 찾아야 할 것이 있나?

## 코딩 Tips
- 제약이 많은 것부터 먼저 구현하는 것이 편리함. (i.e. 데이터 > 계산 > 함수)

## 코드 리뷰 Tips
- 액션을 포함하고 있는 함수도 액션이기 때문에 잘못 구성하면 코드 전체가 액션으로 바뀔 수 있음.
- 액션은 코드에서 다양한 형태로 나타나기 때문에 숨겨진 액션이 있을 수 있음.
    - 함수 호출
    - 메서드 호출
    - 생성자
    - 표현식(변수/속성/배열 참조)
    - 상태(값 할당, 속성 삭제)

## 데이터, 계산, 액션 더 자세히 알아보기

### 데이터
- (구현) 기본 데이터 타입(built-in data types)으로 구현하며, 특정 언어에서는 더 정교한 방법으로 생성 가능함.
- (의미) 데이터 구조는 의미를 담을 수 있는 수단. (e.g. 목록의 순서가 중요하다면, 순서 보장 가능한 데이터 구조 사용)
- (장단점) 전송이 편리하고, 서로 비교가 가능하며 자유로운 해석이 가능하다는 장점이 있으나, 해석이 없다면 쓸모가 없음.

### 계산
- (구현) 함수 형태로 구현
- (의미) 입력값을 출력값으로 만드는 연산을 담을 수 있음.
- (장단점) 테스트와 기계적인 분석이 쉬우며, 조합하기 좋지만, 실행하기 전에는 실제 어떤 일이 발생할지 알기 어려움. 

### 액션
- (구현) 함수 형태로 구현
- (의미) 외부에 영향을 줄 수 있으므로(i.e. 부수 효과가 있으므로) 어떤 일을 하는지 아는 것이 중요함.
- (장단점) 다루기 어렵지만, 소프트웨어를 실행하는 가장 중요한 이유
- 액션을 잘 다루는 Tips
    - 가능한 액션을 적게 사용하기: 액션은 가급적 계산, 데이터로 변경
    - 가능한 액션을 작게 만들기: 액션에서 액션과 관련 없는 코드는 모두 제거
    - 함수형 프로그래밍의 기술들로 외부 세계와의 상호작용과 호출 시점에 의존하는 것을 제한