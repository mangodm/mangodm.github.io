---
title: (KR) 함수형코딩 | 10-일급함수 I
author: mangodm
date: 2022-07-28 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 10장을 읽고, 정리한 페이지

---

- 중복을 없애 추상화를 잘할 수 있는 리팩토링 기법 두 가지를 소개함.

## 암묵적 인자를 드러내기

- 함수 이름에서 구현의 차이점이 드러나는 것을 제외하고 반복적인 패턴이 있다면 암묵적 인자가 존재한다고 할 수 있음.
- 다음의 네 단계를 통해 암묵적 인자를 명시적으로 변경하는 리팩토링을 해볼 수 있음.
    - 함수 이름에 있는 암묵적 인자를 확인
    - 명시적인 인자를 추가
    - 함수 본문에 하드 코딩된 값을 새로운 인자로 변경
    - 함수를 호출하는 곳을 수정


#### (Before)

```jsx
function setPriceByName(cart, name, price) {
	var item = cart[name];
	var newItem = objectSet(item, 'price', price);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}

function setQuantityByName(cart, name, quant) {
	var item = cart[name];
	var newItem = objectSet(item, 'quantity', quant);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}

function setShippingByName(cart, name, ship) {
	var item = cart[name];
	var newItem = objectSet(item, 'shipping', ship);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}
```

#### (After)

```jsx
function setFieldByName(cart, name, field, value) {
	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
} 
```

## 일급인 것과 일급이 아닌 것

- 일급(first-class) 객체는 다음의 네 가지 특징을 가짐.
    - 변수에 할당
    - 함수의 인자로 넘기기
    - 함수의 리턴값으로 받기
    - 배열이나 객체에 담기
- 특히 다른 함수를 인자로 받거나, 리턴 값으로 함수를 리턴할 수 있는 함수는 고차 함수(higher-order function)라고 부름.
- 일급이 아닌 객체들(e.g. 수식 연산자, 반복문 등)을 일급으로 바꾸는 방법을 아는 것이 중요함.

## 용어 정리

- 함수를 정의하는 방법
    - 전역으로 정의하기: 함수를 전역적으로 정의하고, 이름 붙이기
    - 지역적으로 정의하기: 함수를 지역 범위 안에서 정의하고, 이름 붙이기
    - 인라인으로 정의하기(익명 함수): 함수를 사용하는 곳에서 바로 정의
- 콜백(callback): 나중에 호출하기 위해 인자로 전달하는 함수