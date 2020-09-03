---
title: '[Kotlin Cookbook] 다른 기수로 출력하기'
date: 2020-09-03 10:09:27
category: kotlin
thumbnail: { thumbnailSrc }
draft: false
---

### 자바에서 숫자를 이진법으로 출력하고 싶다면?

- 정적 `Integer.toBinaryString` 메소드나 정적 `Integer.toString(int, int)` 메소드 사용
- `Integer.toString(int, int)` 첫번째 인자는 변환할 값, 두번째 인자는 원하는 기수

### 하지만 코틀린에서는?

- 자바의 정적 메소드를 사용해서 Byte, Short, Int, Long에 확장 함수 `toString(radix: Int)`를 만들어 놓음

```kotlin
42.toString(2) == "101010"
```

- 이진 표현에서 비트의 위치는 오른쪽에서 왼쪽으로 1, 2, 4, 8, 16 등에 해당
- 42는 2 + 8 + 32
- 해당 숫자의 비트 위치는 1이고 나머지는 0이기 때문

```kotlin
public actual inline fun Int.toString(radix: Int): String =
  java.lang.Integer.toString(this, checkRadix(radix))
```

- Int에서 toString 메소드의 구현은 다음과 같음
- 정적 메소드 두 번째 인자의 radix을 확인하고 `java.lang.Integer`의 해당 정적 메소드에 위임
- `actual` 키워드는 멀티 플랫폼 프로젝트에서 특정 플랫폼에 종속적인 구현을 나타냄
- radix의 적법한 최소값과 최대값은 각각 2와 36
- `checkRadix` 메소드는 명시한 기수가 위의 범위(2 ~ 36)에 해당하는지 확인
- 범위를 벗어나면 `IllegalArgumentException`을 던짐

```kotlin
val joke = """
  Actually, there are ${3.toString(3)} kinds of people
  Those who know binary, those who don't,
  And those who didn't realize this is actually a ternary joke
""".trimIndent()
println(joke)
```

- 위 내용의 `${3.toString(3)}`의 출력값은 `10`

> 실제로는, 10종류의 사람이 있다
> 이진법을 아는 사람, 이진법을 모르는 사람
> 그리고 이 농담이 실제로 3진(ternary) 농담인지 모르는 사람
