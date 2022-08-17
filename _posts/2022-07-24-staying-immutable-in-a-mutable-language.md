---
title: (KR) 함수형코딩 | 06-불변성 유지하기 I
author: mangodm
date: 2022-07-24 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 6장을 읽고, 정리한 페이지

---

# 06. 변경 가능한 데이터 구조를 가진 언어에서 불변성 유지하기

## 카피 온 라이트란?

- 의도하지 않게 데이터를 변경하는 것(mutable operations)을 막기 위한 방법으로, 아래의 세 단계로 구성됨.
    - 복사본 만들기(make a copy)
    - 복사본 변경하기(modify the copy)
    - 복사본 리턴하기(return the copy)
        
```jsx
// 배열에 대한 카피 온 라이트
function add_element_last(array, elem) {
    var new_array = array.slice();           // 1) 복사본 만들기
    new_array.push(elem);                    // 2) 복사본 변경하기: 배열에 원소 추가
    return new_array;                        // 3) 복사본 리턴하기
}

// 객체에 대한 카피 온 라이트
function setPrice(item, new_price) {
    var item_copy = Object.assign({}, item); // 1) 복사본 만들기
    item_copy.price = new_price;             // 2) 복사본 변경하기
    return item_copy;                        // 3) 복사본 리턴하기
}
```
        
- 함수형 프로그래밍에서 카피 온 라이트는 자주 사용되기 때문에 유틸리티 함수로 생성해두면 편리함.

## 용어 정리

- 중첩 데이터(nested data): 데이터 구조 안에 또 다른 데이터 구조가 있는 형태로 된 데이터
- 핸들러(handler): 특정 이벤트를 처리하도록 생성된 함수를 지칭 (e.g., event handler, exception handler, …)
- 구조적 공유(structural sharing): 두 중첩된 데이터 구조에서 안쪽 데이터가 같은 데이터를 참조함.
- 보일러 플레이트 코드(boilerplate code): 일종의 템플릿처럼 기본이 되는 골격 등을 정의한 코드