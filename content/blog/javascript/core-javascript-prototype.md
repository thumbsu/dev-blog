---
title: '[코어 자바스크립트] 프로토타입 정리'
date: 2020-11-10 17:11:50
category: javascript
thumbnail: { thumbnailSrc }
draft: false
---

### `__proto__`

- 생성자 함수를 `new` 키워드와 함께 호출하면 `Constructor`에서 정의된 내용을 바탕으로 새로운 인스턴스 생성됨
- 이 인스턴스에는 `__proto__`라는, **"`Constructor`의 prototype 프로퍼티를 참조"**하는 프로퍼티가 자동으로 부여
- `__proto__`는 생략 가능한 속성
- 인스턴스는 `Constructor.prototype`의 메서드를 마치 자신의 메서드인 것처럼 호출할 수 있음

### `Constructor.prototype`

- `constructor`라고 다시 생성자 함수 자신을 가리키는 프로퍼티를 가지고 있음
- 자신의 생성자 함수가 무엇인지 알고자 할 때 필요한 수단

### 프로토타입 체이닝

- 직각삼각형의 대각선 방향(`__proto__` 방향)을 계속 찾아가면 최종적으로 `Object.prototype`에 당도함
- 이런 식으로 `__proto__` 안에 다시 `__proto__`를 찾아가는 과정을 프로토타입 체이닝이라고 함
- 각 프로토타입 메서드를 자신의 것처럼 호출할 수 있음
- 접근 방식은 자신으로부터 가장 가까운 대상부터 점차 먼 대상으로 나아가고, 원하는 값 찾으면 검색 중단함
- 무한대의 단계를 생성할 수 있음

### `Object.prototype`

- 모든 데이터 타입에서 사용할 수 있는 범용적인 메서드만이 존재함
- 객체 전용 메서드는 여느 데이터 타입과 달리 `Object` 생성자 함수에 스태틱하게 담겨있음
