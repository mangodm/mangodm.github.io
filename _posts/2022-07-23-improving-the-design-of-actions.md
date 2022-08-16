---
title: (KR) 함수형코딩 | 05-더 좋은 액션 만들기
author: mangodm
date: 2022-07-23 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 5장을 읽고, 정리한 페이지

---

# 05. 더 좋은 액션 만들기

- 더 좋은 액션을 만들기 위해서는 암묵적 입출력을 줄이고, 함수를 최대한 작게 구성해야 함.

## 적을수록 좋은 암묵적 입출력

- 암묵적 입출력이 있는 함수는 의도하지 않은 결과를 발생시킬 수 있어 테스트가 쉽지 않음.
- 암묵적 입출력을 명시적으로 바꿔 모듈화된 컴포넌트로 만들면 테스트 용이성, 재사용성을 높일 수 있음.

### 암묵적 입출력 줄이기

#### (Before)
```jsx
function update_shipping_icons() {
    var buttons = get_buy_buttons_dom();
    for (var i = 0; i < buttons.length; i++) {
        var button = buttons[i];
        var item = button.item;
        var new_cart = add_item(shopping_cart, item.name, item.price);
        if (gets_free_shipping(new_cart)) {
            button.show_free_shipping_icon();
        else
            button.hide_free_shipping_icon();
        }
    }
}

// 함수 호출부
function calc_cart_total() {
    shopping_cart_total = calc_total(shopping_cart);
    set_cart_total_dom();
    update_shipping_icons();
    update_tax_dom();
}
```
#### (After)
```jsx
function update_shipping_icons() {
    var buttons = get_buy_buttons_dom();
    for (var i = 0; i < buttons.length; i++) {
        var button = buttons[i];
        var item = button.item;
        var new_cart = add_item(shopping_cart, item.name, item.price);
        if (gets_free_shipping(new_cart)) {
            button.show_free_shipping_icon();
        else
            button.hide_free_shipping_icon();
        }
    }
}

// 함수 호출부
function calc_cart_total() {
    shopping_cart_total = calc_total(shopping_cart);
    set_cart_total_dom();
    update_shipping_icons();
    update_tax_dom();
}
```

## 설계는 엉켜있는 코드를 푸는 것
- 작은 함수 단위로 코드를 잘 분리하면 다음의 이유에서 좋은 설계라고 할 수 있음.
  - 높은 재사용성
  - 쉬운 유지보수
  - 테스트 용이성 증가
- 함수가 작아지면(e.g. 각 함수가 하나의 일만 한다.) 개념을 중심으로 쉽게 구성할 수 있음.

### 함수를 더 작은 함수로 분리하기
- 함수가 하는 일을 나열 후, 분류해보고 하나의 일만 하도록 변경

#### (Before)
```jsx
function add_item(cart, name, price) {
    var new_cart = cart.slice(); // 1) 배열 복사
    new_cart.push({              // 2) 추가할 item 객체 생성
        name: name,              // 3) 배열 복사본에 item 추가
        price: price
    });
    return new_cart;             // 4) 배열 복사본 리턴
}
```

#### (After)
```jsx
// 1. 추가할 item 객체 생성 분리
function make_cart_item(name, price) {
    return {
        name: name,
        price: price
    };
}

function add_item(cart, item) {
    var new_cart = cart.slice(); // 1) 배열 복사
    new_cart.push(item);
    return new_cart;             // 4) 배열 복사본 리턴
}
// (참고) 함수 호출부 변경
add_item(shopping_cart, make_cart_item("shoes", 3.5));
```

### 재사용이 가능한 (일반적인) 함수로 변환
- 범용적으로 사용될 수 있는 함수를 일반적인 이름으로 만들면, 여러 상황에서 재사용 가능한 유틸리티 함수가 될 수 있음.

#### (Before)

```jsx
function make_cart_item(name, price) {
    return {
        name: name,
        price: price
    };
}

function add_item(cart, item) {
    var new_cart = cart.slice(); // 1) 배열 복사
    new_cart.push(item);
    return new_cart;             // 4) 배열 복사본 리턴
}
```

#### (After)

```jsx
function make_cart_item(name, price) {
    return {
        name: name,
        price: price
    };
}

// 범용적인 함수로 변환
function add_element_last(array, elem) {
    var new_array = array.slice(); // 1) 배열 복사
    new_array.push(elem);
    return new_array;             // 4) 배열 복사본 리턴
}
```

## 용어 정리
- 함수 시그니처(function signature): 함수명, 매개변수, 반환 결과 타입의 조합
- 유틸리티 함수(utility function): 여러 가지 계산과 처리를 대신하는 일반 함수. 높은 재사용성이 특징.