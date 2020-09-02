---
title: 'Dart language - Generics'
date: 2020-08-20 15:05:91
category: dart
thumbnail: { thumbnailSrc }
draft: false
---

- 기본 array type인 List에 대한 API 문서를 보면 type이 실제로 List<E> 인 것을 알 수 있음
- <…> 표기법은 List를 제네릭(또는 parameterized - 매개 변수화된) type, 형식적인 type의 매개 변수를 가진 type으로 표시함
- 일반적으로 대부분의 type 변수에는 E, T, S, K 및 V와 같은 단일 문자 이름이 있음

### **_Why use generics?_**

- 제네릭은 type 안전성을 위해 종종 필요하지만 코드 실행을 허용하는 것보다 더 많은 이점이 있음
  - 제네릭 유형을 올바르게 지정하면 코드가 더 잘 생성됨
  - 코드의 중복을 줄일 수 있음
- 문자열만 포함된 list를 만드려면, list를 List<String> ( "list of string"로 읽음)으로 선언
- 이렇게하면 자신과, 동료 프로그래머 및 toold이 list에 문자열이 아닌 것을 할당하는 실수를 줄일 수 있음

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

- 제네릭을 사용하는 또 다른 이유는 코드 중복을 줄이는 것
- 제네릭을 사용하면 여러 type 간에 단일 인터페이스 및 구현을 공유하면서 정적 분석을 계속 활용할 수 있음
- 아래 코드는 객체 캐싱을 위한 인터페이스를 생성하는 예

```dart
abstract class ObjectCache {
	Object getByKey(String key);
	void setByKey(String key, Object value);
}
```

- 이 인터페이스의 문자열만 받는 버전이 필요하다는 것을 알게 되어 또 다른 인터페이스 생성

```dart
abstract class StringCache {
	String getByKey(String key);
	void setByKey(String key, String value);
}
```

- 숫자 버전이 필요하다고 하면 비슷하지만 또 다른 인터페이스를 생성해야함
- generic type은 이러한 모든 인터페이스를 작성하는 수고를 덜어줌
- type 매개 변수를 사용하는 단일 인터페이스를 만들 수 있음

```dart
abstract class Cache<T> {
	T getByKey(String key);
	void setByKey(String key, T value);
}
```

- 위 코드에서 T는 stand-in type
- 개발자가 나중에 정의할 type으로 생각할 수 있는 placeholder임

#### _**Using Collection literals (컬렉션 리터럴 사용)**_

- List, Set 및 Map 리터럴을 매개 변수화 할 수 있음
- 매개 변수화된 리터럴은 여는 괄호 앞에 <type>(List 및 Set) 또는 <keyType, valueType>(Map)을 추가함

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
	'index.html': 'Homepage',
	'robots.txt': 'Hints for web robots',
	'humans.txt': 'We are people, not machines'
};
```

#### _**Using parameterized types with constructors (생성자에 매개변수화된 type을 사용)**_

- 생성자를 사용할 때 하나 이상의 type을 지정하려면 클래스 이름 바로 뒤에 type을 꺾쇠 괄호 (**<...>**)로 묶음

```dart
var nameSet = Set<String>.from(names);
```

- 아래 코드는 View type의 정수 key와 value가 있는 Map을 만듬

```dart
var views = Map<int, View>();
```

#### _**Generic collections and the types they contain**_

- Dart 제네릭 type이 구체화되었음
- 런타임에 type 정보를 전달
- 예를 들어 컬렉션 유형을 테스트 할 수 있음

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names ad List<String>); // true
```

- 반대로 Java의 제네릭은 삭제를 사용
- 즉, Java는 제네릭 타입 매개 변수가 런타임에 제거됨
- Java에서는 객체가 List인지 여부를 테스트 할 수 있지만 List<String>인지 여부는 테스트 할 수 없음

#### _**Restricting the parameterized type (매개변수화된 type 제한)**_

- generic type을 구현할 때 해당 매개 변수의 type을 제한 할 수 있음
- extends를 사용

```dart
class Foo<T extends SomeBaseClass> {
	// 구현 ...
	String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass { ... }
```

- SomeBaseClass 또는 해당 subclass를 일반 인수로 사용할 수 있음

```dart
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
```

- generic 인수를 지정하지 않아도 됨

```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

- SomeBaseClass가 아닌 type을 지정하면 오류가 발생

#### _**Using generic methods (제네릭 메서드 사용)**_

- 처음에 Dart의 generic 지원은 클래스로 제한되었음
- generic 메서드라고 하는 최신 구문은 메서드 및 함수에 대한 type인수를 허용

```dart
T first<T>(List<T> ts) {
	T tmp = ts[0];
	return tmp
}
```

- first(<T>)의 제네릭 type 매개 변수를 사용하면 여러 위치에서 type 인수 T를 사용할 수 있음
  - function의 리턴 타입(T)
  - 인수의 타입(List<T>)
  - 로컬 변수의 타입(T tmp)
