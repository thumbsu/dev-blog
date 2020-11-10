---
title: '[코어 자바스크립트] 📭 undefined와 null'
date: 2020-11-10 15:11:65
category: javascript
thumbnail: { thumbnailSrc }
draft: false
---

- `undefined`와 `null`은 자바스크립트에서 "없음"을 나타내는 두 가지 값
- 같은 것 같지만 미세하게 다르고, 사용하는 목적 또한 다름

### undefined

- 사용자가 명시적으로 지정
- 값이 존재하지 않을 때 자바스크립트 엔진이 자동으로 부여함

### 자바스크립트 엔진이 자동으로 undefined를 지정하는 경우?

- **사용자가 응당 어떤 값을 지정할 것이라고 예상하는 상황임에도 실제로는 그렇게 하지 않았을 때 undefined를 반환**

```javascript
var a

console.log(a)
```

- 값을 대입하지 않은 변수에 접근하려고 할 때

```javascript
var obj = { a: 1 }

console.log(obj.b)
```

- 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때

```javascript
var func = function() {}
var c = func()

console.log(c)
```

- return문이 없거나 호출되지 않는 함수의 실행 결과

### undefined와 배열

```javascript
var arr1 = []
arr1.length = 3

console.log(arr1) // [empty x 3]

var arr2 = new Array(3)
console.log(arr2) // [empty x 3]

var arr3 = [undefined, undefined, undefined]
console.log(arr3) // [undefined, undefined, undefined]
```

- `arr1`과 `arr2`는 `undefined`조차도 할당돼 있지 않음을 의미
- `arr1`과 `arr2`는 "비어있는 요소"
- "비어있는 요소"와 "undefined를 할당한 요소"는 출력 결과부터 다름
- "비어있는 요소"는 순회와 관련된 많은 배열 메서드들의 **순회 대상에서 제외**
- 어떠한 처리도 하지 않고 건너뜀
- **"배열도 객체"**임을 생각해보면 당연한 현상
- 객체와 마찬가지로 특정 인덱스에 값을 지정할 때 비로소 빈 공간을 확보하고 인덱스를 이름으로 지정하고 데이터의 주소값을 저장하는 등의 동작을 함
- 즉, **값이 지정되지 않은 인덱스는 "아직은 존재하지 않는 프로퍼티"에 지나지 않는 것**

### 사용자가 지정한 undefined와 엔진이 어쩔 수 없이 반환하는 undefined

- 전자의 `undefined`는 그 자체로 값
- "비어있음"을 의미하긴 하지만 하나의 값으로 동작
- **프로퍼티나 배열의 요소는 고유의 키값이 실존**하기 때문에 순회의 대상이 될 수 있음
- 후자는 **해당 프로퍼티 내지 배열의 키값 자체가 존재하지 않음**을 의미
- 후자는 우리의 통제 범위를 벗어나므로 모든 `undefined`가 오직 이 경우만 해당하게끔 해주면 됨

### "비어있음"을 명시적으로 나타내고 싶을 때

- `null`이라는 값이 있는데 굳이 `undefined`를 써야 할 이유가 없음
- `null`은 애초에 이런 용도로 만든 데이터 타입
- 이런 규칙을 따르는 한 **`undefined`는 오직 "값을 대입하지 않은 변수에 접근하고자 할 때 자바스크립트 엔진이 반환해주는 값"으로서만 존재**할 수 있음

### null 주의점

- `typeof null`이 `object`라는 점
- 자바스크립트 자체 버그
- 어떤 변수의 값이 null인지 여부를 판별하기 위해서는 typeof 대신 다른 방식으로 접근해야 함

```javascript
var n = null
console.log(typeof n) // object

console.log(n == undefined) // true
console.log(n == null) // true

console.log(n === undefined) // false
console.log(n === null) // true
```

- 일치 연산자(`===`)를 써야만 정확히 판별됨
