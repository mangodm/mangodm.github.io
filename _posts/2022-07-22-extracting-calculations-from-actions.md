---
title: (KR) 함수형코딩 | 04-액션에서 계산 빼내기
author: mangodm
date: 2022-07-22 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 4장을 읽고, 정리한 페이지

---

# 04. 액션에서 계산 빼내기

- 액션에서 계산을 빼내는 리팩토링을 하면, 테스트하기 쉽고 재사용성이 좋아진다!

## 액션 vs. 계산 구분

- 함수는 입력(계산을 하기 위한 외부 정보)을 사용하여 출력(함수 밖으로 나오는 정보나 어떤 동작/효과)을 반환함.
- 함수에 명시적으로 주어진 입력(인자)과 출력(리턴값) 외의 모든 것은 암묵적인 입출력으로, 부수 효과에 해당함.
- 함수에 암묵적 입출력이 존재하면 액션, 존재하지 않으면 계산이 된다.

## 액션에서 계산 추출

### 서브루틴 추출하기

- 입력값으로 출력값을 만드는 계산에 해당되는 부분을 액션 함수로부터 빼내 별도의 함수로 생성
- 새로 생성한 함수 `calc_total()`은 기계적으로 분리만 된 것으로, 암묵적 입출력이 아직 존재하기 때문에 액션에 해당됨.

#### (Before)
```jsx
var shopping_cart = [];
var shopping_cart_total = 0;

function calc_cart_total() {
    // 0) 장바구니에 담겨 있는 제품의 금액 계산
    shopping_cart_total = 0;
    for(var i = 0; i < shopping_cart.length; i ++) {
        var item = shopping_cart[i];
        shopping_cart_total += item.price;
    }
    // 1) DOM 업데이트
    set_cart_total_dom();
    // 2) 배송 아이콘 업데이트
    update_shipping_icons();
    // 3) 세금 재계산
    update_tax_dom();
}
```
#### (After)
```jsx
var shopping_cart = [];
var shopping_cart_total = 0;

function calc_cart_total() {
    // 0) 새로 만든 함수 호출
    calc_total();
    // 1) DOM 업데이트
    set_cart_total_dom();
    // 2) 배송 아이콘 업데이트
    update_shipping_icons();
    // 3) 세금 재계산
    update_tax_dom();
}

function calc_total() {
    shopping_cart_total = 0;
    for(var i = 0; i < shopping_cart.length; i ++) {
        var item = shopping_cart[i];
        shopping_cart_total += item.price;
    }
}
```

### 입력값은 인자로, 출력값은 리턴값으로 변경
- 전역변수를 읽고, 변경하는 것은 암묵적 입출력이 존재한다는 의미
- 전역변수 대신 지역변수를 사용하도록 바꾸고, 지역변숫값을 리턴하도록 변경해야 함.

#### (Before)

```jsx
var shopping_cart = [];
var shopping_cart_total = 0;

function calc_cart_total() {
    // 0) 장바구니에 담겨 있는 제품의 금액 계산
    shopping_cart_total = 0; // (i) 암묵적 출력
    for(var i = 0; i < shopping_cart.length; i ++) { // (ii) 암묵적 입력
        var item = shopping_cart[i];
        shopping_cart_total += item.price; // (iii) 암묵적 출력
    }
    // 1) DOM 업데이트
    set_cart_total_dom();
    // 2) 배송 아이콘 업데이트
    update_shipping_icons();
    // 3) 세금 재계산
    update_tax_dom();
}
```

#### (After)

```jsx
var shopping_cart = [];
var shopping_cart_total = 0;

function calc_cart_total() {
    // 0) 장바구니에 담겨 있는 제품의 금액 계산
    calc_total(shopping_cart); // shopping_cart를 인자로 전달
    // 1) DOM 업데이트
    set_cart_total_dom();
    // 2) 배송 아이콘 업데이트
    update_shipping_icons();
    // 3) 세금 재계산
    update_tax_dom();
}

function calc_total(cart) { // 전역변수 대신 인자를 만들어 사용
    var total = 0;
    for(var i = 0; i < cart.length; i ++) {
        var item = cart[i];
        total += item.price;
    }
    return total;
}
```

## 용어 정리

- 리팩토링(refactoring): 코드의 동작을 유지하되, 효율성과 유지 보수성을 높이는 작업
- DOM(Document Object Model): 웹브라우저 안에 있는 HTML 페이지를 메모리상에 표현한 것
- 비즈니스 규칙(business rules): 제품이 따라야 할 원칙. 지켜야 할 정부 법규, 산업 표준, 내사 규칙, 알고리즘 등이 해당됨.
- 테스트 커버리지(test coverage): 프로그램의 전체 동작 중에서 테스트로 검사한 범위
- 카피 온 라이트(copy-on-write): 어떤 값을 바꿀 때 그 값을 복사해서 바꾸는 방법, 불변성을 구현하는 방법
- 전역 변수(global variable): 프로그램의 모든 지점에서 호출하고, 사용할 수 있는 변수. 프로그램이 시작될 때 메모리가 할당되고 끝까지 유지되기 때문에 많이 사용할수록 가용 메모리가 줄어듦.
- 지역 변수(local variable): 그 변수가 선언된 함수 혹은 블록 내에서만 사용할 수 있는 변수