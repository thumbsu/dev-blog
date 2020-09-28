---
title: '[Kotlin] Scope Functions 범위 함수'
date: 2020-09-28 15:09:10
category: kotlin
thumbnail: { thumbnailSrc }
draft: false
---

### Scope Functions

- 특정 코드 블럭을 한 객체의 스코프 내에서 실행하고 싶을 때 사용함
- `let`
  - 컨텍스트 객체는 묵시적으로 `it` 이 되며 마지막 표현식의 결과를 반환함
  - `it` 대신 명시적인 변수명 사용 가능
- `apply`
  - 컨텍스트 객체는 `this` 가 되며 컨텍스트 객체 자신을 반환
- `run`
  - 컨텍스트 객체는 `this` 가 되며 마지막 표현식의 결과를 반환함
- `also`
  - 컨텍스트 객체는 `it` 이 되며 컨텍스트 객체 자신을 반환
- `with`
  - 컨텍스트 객체는 `this` 가 되며 마지막 표현식의 결과를 반환
  - 함수의 인자로 객체가 필요하다는 점에서 `run` 과 다름
- 어떤 것을 선택해야 할까?
  - 널이 아닌 객체에 대해 코드 블록 실행: `let`
  - 로컬 범위에서 변수로서의 표현식 실행: `let`
  - 객체 초기화: `apply`
  - 객체를 초기화하면서 결과값을 계산: `run`
  - 표현식이 필요한 실행문: `run`
  - 부수적인 효과: `also`
  - 객체의 함수 호출을 그룹핑: `with`

```kotlin
val a: String? = makeSomeString()?.let {
	// do something
}
```
