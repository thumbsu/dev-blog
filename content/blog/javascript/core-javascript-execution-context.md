---
title: '[코어 자바스크립트] 실행 컨텍스트 정리'
date: 2020-11-10 15:11:85
category: javascript
thumbnail: { thumbnailSrc }
draft: false
---

### 실행 컨텍스트란?

- 실행할 코드에 제공할 환경 정보들을 모아놓은 객체
- 전역 공간에서 자동으로 생성되는 **전역 컨텍스트**, **eval 및 함수 실행에 의한 컨텍스트** 등이 있음

### 실행 컨텍스트 객체가 수집하는 정보

- **VariableEnvironment**, **LexicalEnvironment**, **ThisBinding**의 세 가지 정보를 수집
- 실행 컨텍스트 객체가 **활성화 되는 시점에 수집함**

### VariableEnvironment

- 최초 실행 시의 스냅샷을 유지함
- 현재 컨텍스트 내의 실별자들에 대한 정보와 외부 환경 정보로 구성됨
- 변경사항 반영 안됨

### LexicalEnvironment

- 실행 컨텍스트를 생성할 때, VariableEnvironment에 먼저 정보를 담은 뒤 이를 그대로 복사해서 만듬
- 변경 사항이 실시간으로 반영됨

### VariableEnvironment, LexicalEnvironment 구성 요소

- 매개변수명, 변수의 식별자, 선언한 함수의 함수명 등을 수집하는 environmentRecord
- 바로 직전 컨텍스트의 LexicalEnvironment 정보를 참조하는 outerEnvironmentRecord

### 호이스팅

- 코드 해석을 좀 더 수월하게 하기 위해 **environmentRecord의 수집 과정을 추상화한 개념**
- 실행 컨텍스트가 관여하는 코드 집단의 최상단으로 이들을 "끌어올린다"고 해석하는 것
- 변수 선언과 값 할당이 동시에 이뤄진 문장은 **"선언부"만을 호이스팅**함
- 할당 과정은 원래 자리에 남아있기 때문에, 여기서 **함수 선언문과 함수 표현식**의 차이가 발생

### 스코프

- 변수의 유효범위
- outerEnvironment는 **해당 함수가 선언된 위치의 LexicalEnvironment를 참조**함

#### ❓ 코드 상에서 *어떤 변수*에 접근하려고 하면?

1. 현재 컨텍스트의 LexicalEnvironment를 탐색
2. 발견하면 그 값을 반환
3. 발견하지 못하면 outerEnvironment에 담긴 LexicalEnvironment를 탐색하는 과정을 거침
4. 전역 컨텍스트의 LexicalEnvironment까지 탐색해도 해당 변수를 찾지 못하면 `undefined` 반환

### 전역 변수

- **전역 컨텍스트의 LexicalEnvironment에 담긴 변수**
- 그 외는 모두 지역 변수
- 안전한 코드 구성을 위해 가급적 전역 변수 사용은 최소화하는 것이 좋음

### ThisBinding

- `this` 식별자가 바라봐야 할 대상 객체
- `this`에는 실행 컨텍스트를 활성화하는 당시에 지정된 `this`가 저장
- 함수를 호출하는 방법에 따라 그 값이 달라짐
- 지정되지 않은 경우에는 전역 객체가 저장됨
