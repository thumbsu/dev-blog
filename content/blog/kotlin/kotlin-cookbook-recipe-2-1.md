---
title: '[Kotlin Cookbook] 코틀린에서 널 허용 타입 사용하기'
date: 2020-09-02 11:09:21
category: kotlin
thumbnail: { thumbnailSrc }
draft: false
---

### 변수가 절대로 널(null)값을 갖지 못하게 하려면?

- 안전 호출 연산자(`?.`)나 엘비스 연산자(`?:`)와 결합해서 사용
- 널 값이더라도 중간 이름인 middle 파라미터에 값을 제공해야 함

```kotlin
class Person(val first: String, val middle: String?, val last: String)

val jkRowling = Person("Joanne", null, "Rowling")
val northWest = Person("North", null, "West")
```

### val 변수의 널 허용성 검사

- null 할당이 불가능한 문자열 타입으로 영리한 타입 변환(smart cast)을 수행
- `if`문은 `middle` 속성이 널이 아닌 값을 가지고 있는지 확인

```kotlin
val p = Person(first = "North", middle = null, last = "West")
if (p.middle != null) {
  val middleNameLength = p.middle.length
}
```

### `middle`값이 널이 아니라면?

- 마치 `p.middle`의 타입이 `String?` 타입 대신 `String` 타입인 것처럼 처리하는 영리한 타입 변환 수행
- 영리한 타입 변환(`smart cast`)를 수행할 수 있는 이유는?
- 변수 `p`가 한번 설정되면 그 값을 바꿀 수 없는 `val` 키워드로 선언되었기 때문

### var 변수가 널 값이 아님을 단언하기

- `m.middle`이 복합식(`complex expression`)이므로 영리한 타입 변환 불가능

```kotlin
var p = Person(first = "North", middle = null, last = "West")
if (p.middle != null) {
  val middleNameLength = p.middle!!.length
}
```

- 널이 아님을 단언함(`not-null assertion operator`)
- `p`는 `val` 대신 `var`를 사용하기 때문에 변수 `p`가 정의된 시점과 `p`의 `middle` 속성에 접근하는 시점 중간에 값이 변경되었을 수 있다고 가정됨
- 따라서 영리한 타입 변환 수행 안함
- 영리한 타입 변환이 수행되도록 우회하는 방법?
- `not-null assertion operator`라고 부르는 거듭 느낌표(`!!`) 사용
- 근데 이게 하나라도 있으면 코드 스멜(`code smell`; 잠재적으로 문제가 있는 코드)
- `!!` 연산자는 변수가 널 아닌 값으로 다뤄지도록 강제함
- 해당 변수가 널이라면 예외를 던짐
- 코틀린에서 `NullPointerException`을 만날 수 있는 몇 가지 상황 중 하나
- **가급적 사용하지 말자**

### 안전 호출 연산자(`?.`) 사용하기

- 위와 같은 상황에서는 안전 호출 연산자를 사용하는 것이 좋음
- 안전 호출은 쇼트 서킷(`short-circuit`) 평가 방식이며 널 값이면 null을 돌려줌

```kotlin
var p = Person(first = "North", middle = null, last = "West")
val middleNameLength = p.middle?.length
```

- 위 코드의 결과 타입은 `Int?`

### 안전 호출 연산자와 엘비스 연산자 병행하기

- 위 코드의 문제는 결과 추론 타입도 널 허용 타입이라는 점
- 따라서 middleNameLength의 타입은 사용하려했던 타입이 아니라 `Int?` 이므로 안전 호출 연산자와 엘비스 연산자를 병행해서 사용하는 것이 유용함

```kotlin
var p = Person(first = "North", middle = null, last = "West")
val middleNameLength = p.middle?.length ?: 0
```

- `middle`이 널일 경우 엘비스 연산자는 0을 리턴함
- 엘비스 연산자는 자신의 왼쪽에 위치한 식의 값을 확인함
- 해당 값이 널이 아니면 그 값을 리턴하고 널이면 오른쪽 값을 리턴함
- 엘비스 연산자의 오른쪽은 식이므로 함수 인자를 확인할 때 return이나 throw를 사용할 수 있음

### 안전 타입 변환 연산자

- 코틀린은 안전 타입 변환를 제공함
- 목적은 타입 변환이 올바르게 동작하지 않는 경우 ClassCastException이 발생하는 상황을 방지하는 것
- 예: 어떤 인스턴스를 `Person` 타입으로 변환을 시도했지만 해당 인스턴스가 널일 수 있는 경우

```kotlin
val p1 = p as? Person
```

- 변수 `p1`의 타입은 `Person?`
- 타입 변환은 **성공**하여 `Person` 타입이 되던지 **실패**하여 p1이 널 값을 받음
