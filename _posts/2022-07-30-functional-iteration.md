---
title: (KR) 함수형코딩 | 12-함수형 반복
author: mangodm
date: 2022-07-30 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 12장을 읽고, 정리한 페이지

---

# 12. 함수형 반복

- 함수형 프로그래밍 언어에는 컬렉션 데이터를 다루는 강력한 추상 함수들이 존재함.
- `map()`, `filter()`, `reduce()`가 대표적으로, 배열을 반복해서 처리하는데 탁월하여 자주 사용됨.

## map()
- X 값이 있는 배열에 함수를 적용하여 Y 값이 있는 배열로 변환함.
- 리턴값인 결과 배열에 들어 있는 항목은 확인하지 않기 때문에 NULL이나 undefined가 포함될 수 있으니 주의 필요
```javascript
function map(array, f) {
    var new_array = []; // 빈 배열 생성
    forEach(array, function(element) {
        new_array.push(f(element)); // 원래 배열 항목에 f 함수를 적용하여 새로운 배열에 추가
    });
    return new_array; // 새로운 배열을 리턴
}
```

## filter()
- 함수를 활용하여 X 값이 있는 배열 중, 특정 조건을 만족하는 X 값들만 남긴 배열을 반환함.
- 조건을 판단하는데 활용되는 함수를 보통 술어(predicate)라고 부름.
```javascript
function filter(array, f) {
    var new_array = []; // 빈 배열 생성
    forEach(array, function(element) {
        if(f(element))             // 원래 배열 항목에 조건을 판단하는 f 함수를 적용
            new_array.push(element); // 조건에 해당되는 항목만 결과 배열에 추가
    });
    return new_array; // 결과 배열을 리턴
}
```

## reduce()
- 함수를 활용하여 X 값이 있는 배열의 항목에 누적적으로 어떤 동작을 취한 뒤, 최종 결과값을 반환함.
- 초깃값을 잘못 설정하면 누적의 결과가 의도하지 않게 바뀔 수 있으므로 맥락을 고려하여 신중하게 설정해야 함.
    - (참고) 초깃값을 결정하는 방법
        - 계산이 어떤 값에서 시작되는지(e.g. 더하기(+): 초깃값 0, 곱하기(*): 초깃값 1)
        - 빈 배열을 사용한다면 어떤 값을 리턴할 것인지(e.g. 빈 문자열을 합친다: 빈 문자열 반환)
        - 비즈니스 규칙은 무엇인지
- 특정 시점의 상태 값을 보관할 수 있기 때문에 실행 취소/복귀, 디버깅 등 다양한 곳에 활용될 수 있음.
```javascript
function reduce(array, init, f) { // 배열, 초깃값, 누적적인 동작을 정의한 함수
    var accum = init;
    forEach(array, function(element) { 
        accum = f(accum, element); // 누적 값을 계산하기 위해 현재 값과 배열의 항목을 인자로 전달
    });
    return accum; // 누적된 값을 리턴
}
```