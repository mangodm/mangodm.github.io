---
title: (KR) 함수형코딩 | 17-타임라인 조율하기
author: mangodm
date: 2022-08-04 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 17장을 읽고, 정리한 페이지

---

- 동시성 기본형을 활용하여 타임라인을 관리하는 재사용 가능 객체를 만들 수 있음.

## Cut() 함수

- 타임라인을 분리하여 액션이 섞이지 않고, 순서가 보장되도록 하는 기법
- 실행 가능한 순서가 중요하지 않은 콜백들은 병렬로 처리 후, 컷을 적용하면 실행 속도를 개선할 수 있음.
    
```jsx
function Cut(num, callback) { // num: 기다릴 타임라인의 수, callback: 모든 것이 끝났을 때 실행할 콜백
    var num_finished = 0; // 카운터 초기화
    return function() { // 타임라인이 끝났을 때 호출
        num_finished += 1; // 함수를 호출할 때마다 카운터가 증가
        if (num_finished === num)
            callback(); // 마지막 타임라인이 끝났을 때 호출
    };
}
```
    
## Justonce() 함수

- 여러 번 호출해도 한번만 실행하도록 해주는 기법
    
```jsx
function JustOnce(action) {
    var alreadyCalled = false;
    if (alreadyCalled) // 실행한 적이 있다면 바로 종료
        return;
    alreadyCalled = true;
    return action(a, b, c); // 인자와 함께 액션 호출
    };
}
```
    

## 암묵적 시간 모델 vs. 명시적 시간 모델

- 각 언어마다 암묵적으로 실행에 관한 시간 모델(순서, 반복)을 갖고 있음.
    - (예) 자바 스크립트의 시간 모델
        - (순서) 순차적 구문은 순서대로 실행
        - (순서) 두 타임라인에 있는 단계는 왼쪽 먼저 실행되거나, 오른쪽 먼저 실행(멀티 스레드 지원 X)
        - (순서) 비동기 이벤트는 새로운 타임라인에서 실행
        - (반복) 액션은 호출될 때마다 실행
- 언어의 시간 모델이 원하는 것과 다를 수 있기 때문에 애플리케이션에 적합한 시간 모델을 명시적으로 만들 수 있어야 함.