---
title: (KR) 함수형코딩 | 11-일급함수 II
author: mangodm
date: 2022-07-29 11:33:00 +0800
categories: [CS, functional-programming]
tags: [kr, grokking-simplicity]
math: true
---

에릭 노먼드의 [쏙쏙 들어오는 함수형 코딩](http://www.yes24.com/Product/Goods/108748841) 11장을 읽고, 정리한 페이지

---

## 함수 본문을 콜백으로 바꾸기

- 함수 본문에 반복적으로 나타나는 패턴, 원칙 등을 추상화하여 별도의 함수(i.e. 콜백)로 생성하고, 인자로 전달
- 다음의 세 단계로 리팩토링을 해볼 수 있음.
    - 본문과 그 앞부분, 뒷부분 확인
    - 함수 빼내기
    - 콜백 빼내기
- 고차 함수로 중복 코드를 많이 없앨 수 있으나, 지나치게 사용하면 가독성이 떨어질 수 있기 때문에 적절한 곳에 사용해야 함.

#### (Before)

```jsx
function arraySet(array, idx, value) {
	var copy = array.slice(); // 앞부분 > 함수
	copy[idx] = value;        // 본문 > 콜백
	return copy;              // 뒷부분 > 함수
}

function push(array, elem) {
	var copy = array.slice();
	copy.push(elem);
	return copy;
}
```

#### (After)

```jsx
function arraySet(array, idx, value) {
	return withArrayCopy(array, function(copy) {
		copy[idx] = value;
});
}

function push(array, elem) {
	return withArrayCopy(array, function(copy) {
		copy.push(elem);
});
}

function withArrayCopy(array, modify) { // modify: 여러 변경 동작에 대한 일반적인 이름
	var copy = array.slice();
	modify(copy);
	return copy;
}
```

