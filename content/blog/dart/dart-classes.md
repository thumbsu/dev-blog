---
title: 'Dart - Classes'
date: 2020-09-02 16:09:24
category: dart
thumbnail: { thumbnailSrc }
draft: false
---

### **_Instance variables (인스턴스 변수)_**

```dart
class Point {
  double x;
  double y;
  double z = 0;
}

void main(List<String> arguments) {
  var point = Point();
  point.x = 4; // x의 setter method를 사용함
  print(point.x == 4); // x의 getter method를 사용함
  print(point.y == null); // default 값은 null
}
```

- 초기화되지 않은 모든 인스턴스 변수의 값은 null
- 모든 인스턴스 변수는 암시적 getter 메서드를 생성함
- 최종 인스턴스 변수가 아닌 경우에도 암시적 setter 메서드가 생성됨

### **_Constructor (생성자)_**

```dart
class Point {
  double x;
  double y;

  Point(double x, double y) {
    this.x = x;
    this.y = y;
  }
}
```

- 클래스와 이름이 같은 함수를 생성하여 생성자를 선언
- 선택적으로 Named Constructors에 설명 된 추가 식별자도 포함
- 생성자의 가장 일반적인 형태인 generative constructor는 클래스의 새 인스턴스를 만듬
- `this` 키워드는 현재 인스턴스를 참조함

```dart
class Point {
  double x;
  double y;

  Point(double a, double b) {
    x = a;
    y = b;
  }
}
```

- `this` 키워드는 이름에 충돌이 있는 경우에만 사용함
- 그렇지 않을 때는 `this` 를 생략하는 게 Dart 스타일

```dart
class Point {
  double x;
  double y;

  Point(this.x, this.y);
}
```

- 생성자 인수를 인스턴스 변수에 할당하는 패턴은 매우 일반적
- 위의 구문처럼 사용 가능 (body 없음)

**_Default constructors (기본 생성자)_**

- 생성자를 선언하지 않으면 default constructor가 제공됨
- default constructor에는 인수가 없으며 superclass에서 인수가 없는 생성자를 호출함

**_Constructors arent's inherited (생성자는 상속되지 않음)_**

- subclass는 superclass에서 생성자를 상속하지 않음
- 생성자를 선언하지 않는 subclass에는 default(인수, 이름 없음) constructor만 있음

**_Named constructors (명명 된 생성자)_**

- 여러 생성자를 구현할 수 있음
- 좀 더 명확하게 할 수 있음

```dart
class Point {
  double x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```

- 생성자는 상속되지 않음
- 즉, superclass의 named constructor가 subclass에서 상속되지 않음
- superclass에 정의된 named constructor로 subclass를 생성하려면 해당 생성자를 subclass에서 구현해야 함

**_Invoking a non-default superclass constructor (기본이 아닌 슈퍼 클래스 생성자 호출)_**

- 기본적으로 subclass의 생성자는 수퍼 클래스의 명명되지 않은 인수없는 생성자를 호출함
- superclass의 생성자는 생성자 body의 시작 부분에서 호출됨
- initializer list도 사용중인 경우 superclass가 호출되기 전에 실행됨
- 요약하면 실행 순서는 다음과 같음
  1. Initializer list
  2. superclass의 인수 없는 생성자
  3. main class의 인수 없는 생성자
- superclass에 이름이 지정되지 않은 인수 없는 생성자가 없으면 superclass의 생성자 중 하나를 수동으로 호출해야함
- 생성자 body가 있는 경우 body 바로 앞에, 콜론 (:) 뒤에 superclass 생성자를 지정함

```dart
class Person {
	String firstName;

	Person.fromJson(Map data) {
		print('in Person');
	}
}

class Employee extends Person {
	// Person class는 기본 생성자가 없음
	// 따라서 무조건 super.fromJson(data)를 호출해야함
	Employee.fromJson(Map data) : super.fromJson(data) {
		print('in Employee');
	}
}

main() {
	var emp = new Employee.fromJson({});

	// 출력:
	// in Person
	// in Employee

	if (emp is Person) {
		emp.firstName = 'Bob';
	}
	(emp as Person).firstName = 'Bob';
```

- superclass 생성자에 대한 인수는 생성자를 호출하기 전에 평가됨
- 따라서 인수는 함수 호출과 같은 표현식이 될 수 있음

```dart
class Employee extends Person {
	Employee() : super.fromJson(defaultData);
}
```

- superclass 생성자에 대한 인수는 `this` 에 접근할 수 없음
- 예를 들어, 인수는 인스턴스 메서드가 아닌 정적 메서드를 호출 할 수 있음

**_Initializer list (초기화 목록)_**

- 생성자 본문이 실행되기 전에 인스턴스 변수를 초기화 할 수 있음
- initializer는 쉼표로 구분함

```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, double> json)
		: x = json['x'],
		y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```

- initializer의 오른쪽(ex. `json['x']`)은 `this` 에 접근할 수 없음
- 개발 중에 initializer list에 `assert`를 추가해서 유효성 검사를 할 수 있음

```dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
	print('In Point.withAssert(): ($x, $y)');
}
```

- initializer list는 `final` 필드를 설정할 때 편리함
- 다음 예제는 initializer list에서 세 개의 `final` 필드를 초기화함

```dart
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
	var p = dart1.Point(2, 3);
  print(p.distanceFromOrigin);

	// 출력:
	// 3.605551275463989
}
```

**_Redirecting constructors (리다이렉션 생성자)_**

- 동일한 클래스의 다른 생성자로 리디렉션할 수 있음
- 리디렉션 생성자의 본문이 비어 있으며 생성자 호출이 콜론 (:) 뒤에 표시됨

```dart
class Point {
	double x, y;

	Point(this.x, this.y);

	Point.alongXAxis(double x) : this(x, 0);
}
```

**_Constant constructors (상수 생성자)_**

- 클래스가 변경되지 않는 객체를 생성하는 경우 이러한 객체를 compile-time 상수로 만들 수 있음
- `const` 생성자를 정의하고 모든 인스턴스 변수가 `final` 변수여야함

```dart
class ImmutablePoint {
	static final ImmutablePoint origin =
		const ImmutablePoint(0, 0);

	final double x, y

	const ImmutablePoint(this.x, this.y);
}
```

- `const` 생성자가 늘 상수를 만드는 것은 아님

```dart
var a = const ImmutablePoint(1, 1); // 상수
var b = ImmutablePoint(1, 1); // 상수 아님

assert(!identical(a, b)); // 같은 인스턴스 아님
```

- `const` 생성자가 `const` 컨텍스트 외부에 있고 const없이 호출되면 상수가 아닌 객체를 만듬

**_Factory constructors_**

- 항상 클래스의 새 인스턴스를 만들지는 않는 생성자를 구현할 때 factory 키워드를 사용함
- 예를 들어 factory 생성자는 캐시에서 인스턴스를 반환하거나 하위 유형의 인스턴스를 반환 할 수 있음

```dart
class Logger {
  final String name;
  bool mute = false;

  static final Map<String, Logger> _cache = <String, Logger>{};

  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

- factory constructor는 `this` 에 접근할 수 없음

### Methods

- 객체에 대한 동작을 제공하는 함수

**_Instance methods (인스턴스 메소드)_**

- 객체의 인스턴스 메서드는 인스턴스 변수 및 `this`에 접근할 수 있음

```dart
import 'dart:math';

class Point {
	double x, y;

	Point(this.x, this.y);

	double distanceTo(Point other) {
		var dx = x - other.x;
		var dy = y - other.y;
		return sqrt(dx * dx + dy * dy);
	}
}
```

**_Getters and setters_**

- Getter 및 Setter는 개체 속성에 대한 읽기 및 쓰기 접근할 수 있게 해주는 특수 메서드
- 각 인스턴스 변수에는 암시적 getter와 적절한 경우 setter가 있음
- get 및 set 키워드를 사용하여 getter 및 setter를 구현하여 추가 속성을 만들 수 있음

```dart
class Rectangle {
	double left, top, width, height;

	Rectangle(this.left, this.top, this.width, this.height);

	double get right => left + width;
	set right(double value) => left = value - width;
	double get bottom => top + height;
	set bottom(double value) => top = value - height;
}

void main() {
	var rect = Rectangle(3, 4, 20, 15);
	assert(rect.left == 3);
	rect.right = 12;
	assert(rect.left == -8);
}
```

- getter 및 setter를 사용하면 인스턴스 변수로 시작하고 나중에 클라이언트 코드를 변경 없이 메서드로 감쌀 수 있음
- 증가(`++`)와 같은 연산자는 getter가 명시적으로 정의되었는지 여부에 관계없이 예상된 방식으로 작동함
- 예기치 않은 부작용을 방지하기 위해 연산자는 getter를 정확히 한 번 호출하여 해당 값을 임시 변수에 저장함

**_Abstract methods_**

- 인스턴스, getter 및 setter 메서드는 추상적일 수 있음
- 인터페이스를 정의하지만 구현은 다른 클래스에 맡김
- 추상 메서드는 추상 클래스에만 존재할 수 있음

```dart
abstract class Doer {
	void doSomething();
}

class EffectiveDoer extends Doer {
	void doSomething() {
		// 실제 구현
	}
}
```

###

### Abstract classes (추상 클래스)

- `abstract` 수식어구를 사용하여 인스턴스화 할 수 없는 추상 클래스를 정의함
- 추상 클래스는 종종 일부 구현과 함께 인터페이스를 정의하는 데 유용함
- 추상 클래스가 인스턴스화 가능한 것처럼 보이게 하려면 팩토리 생성자를 정의

```dart
abstract class AbstractContainer {
	void updateChildren(); // 추상 메소드
}
```

### Implicit interfaces (암시적 인터페이스)

- 모든 클래스는 인터페이스를 암시적으로 정의함
- 클래스의 모든 인스턴스 멤버와 구현하는 인터페이스를 포함
- B의 구현을 상속하지 않고 클래스 B의 API를 지원하는 클래스 A를 만들려면, 클래스 A가 B 인터페이스를 구현해야 함

```dart
// Person 클래스
// 암시적 인터페이스에는 greet() 메소드를 포함함
class Person {
	// 인터페이스에 있지만 이 라이브러리에서만 볼 수 있음
	final _name;

	// 생성자이기 때문에 이 인터페이스에 없음
	Person(this._name);

	// 인터페이스에서
	String greet(String who) => 'Hello, $who. I am $_name';
}

// Person 인터페이스를 구현
class Impostor implements Person {
	get _name => '';

	String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
	print(greetBob(Person('Kathy')));
	print(greetBob(Imposteor()));

	// 출력:
	// Hello, Bob. I am Kathy
	// Hi Bob. Do you know who I am?
}
```

- 다음은 여러 인터페이스를 구현하는 클래스의 예시

```dart
class Point implements Comparable, Location {...}
```

### Extending a class (클래스 확장)

- `extends` 를 사용하여 subclass를 만들고 `super`를 사용하여 superclass를 참조함

```dart
class Television {
	void turnOn() {
		_illuminateDisplay();
		_activateIrsensor();
	}
	// ...
}

class SmartTelevision extends Television {
	void turnOn() {
		super.turnOn();
		_bootNetworkInterface();
		_upgradeApps();
	}
}
```

**_Overriding members_**

- subclass는 인스턴스 메서드, getter 및 setter를 재정의 할 수 있음
- `@override` 어노테이션을 사용하여 의도적으로 멤버를 대체하고 있음을 나타낼 수 있음

```dart
class SmartTelevision extends Television {
	@override
	void turnOn() {...}
	// ...
}
```

**_Overridable operators_**

- 아래 리스트의 연산자를 재정의 할 수 있음

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| <   | -   | \*  | &   | []= |
| >   | +   | %   | <<  | ~   |
| >=  | /   | \|  | >>  | ==  |
| <=  | ~/  | ^   | []  |     |

- 예를 들어 Vector 클래스를 정의하는 경우, + 메서드를 정의하여 두 개의 벡터를 추가 할 수 있음
- `!=` 는 재정의 할 수 없음
- `e1 != e2` 표현식은 `!(e1 == e2)` 의 축약형
- 아래는 `+` 와 `-` 연산자를 재정의 하는 클래스의 예

```dart
class Vector {
	final int x, y;

	Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
	Vector operator -(Vector v) => Vector(x - v.x, y - v.y);
}

void main() {
	final v = Vector(2, 3);
	final w = Vector(2, 2);

	assert(v + w == Vector(4, 5));
	assert(v - w == Vector(0, 1));
}
```

- `==`를 재정의하는 경우 Object의 `hashCode` getter도 재정의 해야 함
- Dart에서 `==` 연산자를 사용하면 `hashCode`가 같은지를 확인 함

**_noSuchMethod()_**

- 코드가 존재하지 않는 메서드 또는 인스턴스 변수를 사용하려고 할 때마다 감지하거나 반응하려면 `noSuchMethod()`를 재정의 할 수 있음

```dart
class A {
	@override
	void noSuchMethod(Invocation invocation) {
		print('You tried to use a non-existent member: ' + '${invocation.memberName}');
	}
}
```

- 다음 중 하나에 해당하지 않으면 구현되지 않은 메서드를 호출 할 수 없음
  - receiver는 정적 타입인 `dynamic` 이어야 함
  - receiver에는 구현되지 않은 메서드를 정의하는 `static type`이 있고, recevier의 `dynamic type`에는 `class` 객체의 것과 다른 `noSuchMethod()` 구현이 있음

### Extension methods (확장 메소드)

- Dart 2.7에 도입된 확장 메서드는 기존 라이브러리에 기능을 추가하는 방법
- 모르는 사이에 extension methods을 사용할 수 있음
- 예를 들어 IDE에서 코드 완성을 사용하면 일반 메서드와 함께 확장 메서드를 제안함
- 다음은 `string_apis.dart`에 정의된 `parseInt()`라는 `String`의 extension method를 사용하는 예

```dart
import 'string_apis.dart'

print('42'.padLeft(5)); // String method 사용
print('42'.parseInt()); // extension method 사용
```

### Enumerated types (Enum 타입)

- enumerations 또는 enums라고 하는 Enumerated Type은 고정된 수의 상수 값을 나타내는 데 사용되는 특수한 종류의 클래스

**_Using enums_**

- `enum` 키워드를 사용하여 Enumerated type을 선언함

```dart
enum Color { red, green, blue }
```

- enum의 각 값에는 enum 선언에서 값의 0부터 시작하는 위치를 반환하는 `index` getter가 있음
- 예를 들어 첫 번째 값에는 `index` 0이 있고 두 번째 값에는 `index` 1이 있음

```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

- enum의 모든 값 목록을 가져 오려면 enum의 값 상수를 사용함

```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

- `switch` 문에서 enum type을 사용할 수 있음
- enum type의 모든 값을 처리하지 않으면 경고가 표시됨

```dart
var aColor = Color.blue;

switch (aColor) {
	case Color.red:
		print('Red as roses!');
		break;
	case Color.green:
		print('Green as grass!');
		break;
	default: // 이게 없으면 WARNING이 뜸
		print(aColor); // Color.blue
}
```

- enumerated type에는 다음과 같은 제한이 있음
  - enum을 subclass화 하거나, mix 하거나 구현할 수 없음
  - enum을 명시적으로 인스턴스화 할 수 없음

### Adding features to a class: mixins

- Mixin은 여러 class 계층에서 class의 코드를 재사용하는 방법
- Mixin을 사용하려면 `with` 키워드 다음에 하나 이상의 mixin 이름을 사용

```dart
class Musician extends Performer with Musical {
	// ...
}

class Maestro extends Person with Musical, Aggressive, Demented {
	Maestro(String maestroName) {
		name = maestroName;
		canConduct = true;
	}
}
```

- mixin을 구현하려면 Object를 확장하고 생성자를 선언하지 않는 클래스를 만듬
- mixin을 일반 class로 사용하려면 class 대신 mixin 키워드를 사용함

```dart
mixin Musical {
	bool canPlayPiano = false;
	bool canCompose = false;
	bool canConduct = false;

	void entertainMe() {
		if (canPlayPiano) {
			print('Playing piano');
		} else if (canConduct) {
			print('Waving hands');
		} else {
			print('Huming to self');
		}
	}
}
```

- 특정 type만 mixin을 사용할 수 있도록 지정하려면 (ex. mixin이 정의하지 않은 메서드를 호출 할 수 있도록) `on`을 사용하여 필요한 superclass를 지정

```dart
mixin MusicalPerformer on Musician {
	// ...
}
```

### Class variables and methods

- `static` 키워드를 사용하여 클래스 전체 변수 및 메서드를 구현

**_Static variables_**

- static 변수(클래스 변수)는 클래스 전체 상태 및 상수에 유용함

```dart
class Queue {
	static const initialCapacity = 16;
	// ...
}

void main() {
	assert(Queue.initialCapacity == 16);
}
```

- static 변수는 사용될 때까지 초기화되지 않음

**_Static methods_**

- static 메서드(클래스 메서드)는 인스턴스에서 작동하지 않으므로 이에 대한 접근 권한이 없음

```dart
import 'dart:math';

class Point {
	double x, y;
	Point(this.x, this.y);

	static double distanceBetween(Point a, Point b) {
		var dx = a.x - b.x;
		var dy = a.y - b.y;
		return sqrt(dx * dx + dy * dy);
	}
}

void main() {
	var a = Point(2, 2);
	var b = Point(4, 4);
	var distance = Point.distanceBetween(a, b);
	assert(2.8 < distance && distance < 2.9);
	print(distance);
	// 출력:
	// 2.8284271247461903
}
```

- static 메서드를 compile-time 상수로 사용할 수 있음
- 예를 들어 static 메서드를 상수 생성자에 매개 변수로 전달할 수 있음
