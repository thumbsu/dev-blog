---
title: '[Kotlin Cookbook] 명시적으로 타입 반환하기'
date: 2020-09-02 15:09:92
category: kotlin
thumbnail: { thumbnailSrc }
draft: false
---

### 기본 타입을 더 넓은 타입으로 승격시키려면?

- 코틀린은 Int를 Long으로 자동 승격시키지 않음
- 더 작은 타입을 명시적으로 변환하려면 구체적인 변환 함수 사용함
- 예: `toInt`, `toLong`
- 반면, 자바는 더 짧은 타입이 자동으로 더 긴타입으로 승격됨

```java
int myInt = 3;
long myLong = myInt;
```

- 자바 1.5에서 오토박싱(Autoboxing)이 등장
- 기본 타입을 래퍼(wrapper) 타입으로 변한하기는 쉬워짐
- 한 래퍼 타입에서 다른 래퍼 타입으로 변환하려면 추가 코드가 필요함

```java
Integer myInteger = 3;
// Long myWrappedLong = myInteger; -> 컴파일 되지 않음
Long myWrappedLong = myInteger.longValue(); // #1
myWrappedLong = Long.valueOf(myInteger); // #2
```

- `#1`: `long`으로 추출한 다음 래퍼 타입으로 감쌈
- `#2`: 래퍼 타입을 벗겨 int 타입을 얻고 long으로 승격시킨 다음에 다시 랩

> #### 오토박싱(Autoboxing)이란?
>
> - 자바 컴파일러가 원시 데이터 타입을 그에 상응하는 Wrapper 클래스로 자동 변환 시켜주는 것
> - `int` -> `Integer`
> - `double` -> `Double`

- 래퍼 타입을 직접 다루는 것은 언박싱을 개발자 스스로 해야 한다는 것
- 먼저 포장된 값을 추출하는 작업 없이는 간단하게 `Integer` 인스턴스를 `Long`에 할당할 수 없음

### 코틀린에서는?

- 기본 타입을 직접적을 제공하지 않음
- 바이트코드에서는 해당되는 기본 타입을 생성하겠지만 개발자가 코드를 직접 작성할 때는 기본 타입이 아닌 **_클래스_**를 다룸
- 코틀린에서는 `toInt`, `toLong`과 같은 변환 메소드 제공

### 코틀린에서 Int를 Long으로 승격시키기

- 변환 메소드를 사용하여 명시적으로 타입을 변환함

```kotlin
val intVar: Int = 3
// val longVar: Long = intVar -> 컴파일되지 않음
val longVar: Long = intVar.toLong()
```

### 사용 가능한 타입 변환 메소드

- `toByte()` : Byte
- `toChar()` : Char
- `toShort()` : Short
- `toInt()` : Int
- `toLong()` : Long
- `toFloat()` : Float
- `toDouble()` : Double

### 명시적 타입 변환이 필요하지 않을 때도 있음

```kotlin
val longSum = 3L + intVar
```

- 더하기(+) 연산자는 자동으로 intVar의 값을 Long으로 변환시킴
- Long 리터럴에 Long으로 변환시킨 값을 더함
