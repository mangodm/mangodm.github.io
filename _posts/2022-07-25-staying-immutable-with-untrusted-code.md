---
title: (KR) 함수형코딩 | 07-불변성 유지하기 II
author: mangodm
date: 2022-07-25 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 7장을 읽고, 정리한 페이지

---

# 07. 신뢰할 수 없는 코드를 쓰면서 불변성 지키기

## 방어적 복사(defensive copying)

- 세부적인 동작을 파악하거나, 통제하기 어려운 코드를 사용하면 카피 온 라이트 패턴을 적용하기 어려움.
- 방어적 복사는 깊은 복사 방식을 활용하여 데이터가 바뀌는 것을 완벽히 방지하는 원칙으로 다음의 방법을 활용함.
1. 데이터가 안전한 코드(i.e. 데이터의 불변성이 지켜지는 코드)에서 나갈 때 복사하기
      - 데이터가 바뀌지 않도록 깊은 복사본을 생성
      - 신뢰할 수 없는 코드로 복사본을 전달
2. 안전한 코드로 데이터가 들어올 때 복사하기
      - 변경될 수 있는 데이터가 들어오면 바로 깊은 복사본을 생성하여 안전한 코드로 전달
      - 복사본을 안전한 코드에서 사용

```jsx
function add_item_to_cart(name, price) {
    var item = make_cart_item(name, price);
    shopping_cart = add_item(shopping_cart, item);
    var total = calc_total(shopping_cart);
    set_cart_total_dom(total);
    update_shipping_icons(shopping_cart);
    var cart_copy = deepCopy(shopping_cart);  // (비안전지대)로 넘기기 전에 복사
    black_friday_promotion(cart_copy);
    shopping_cart = deepCopy(cart_copy);      // (안전지대)로 들어오기 전에 복사
}
```
    
- 방어적 복사에서 활용되는 깊은 복사는 원본의 모든 것(중첩된 객체, 배열 등)을 복사하여 비용이 많이 들기 때문에 카피 온 라이트를 적용하기 어려운 경우에만 사용하는 것이 좋음.

## 용어 정리

- 레거시 코드(legacy code): 오래전에 만든 것으로, 지금 당장 고칠 수 없어서 그대로 사용해야 하는 코드
- 얕은 복사(shallow copy): 데이터 구조의 최상위 단계만 복사
- 깊은 복사(deep copy): 모든 계층에 있는 중첩된 데이터 구조를 복사