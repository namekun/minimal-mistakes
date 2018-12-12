---
title: "LearningJS-CH02"
tags:
  - Javascript
comments: true
---

# 리터럴과 변수, 상수, 데이터 타입

### 데이터를 자바스크립트가 이해할 수 있는 형식으로 바꾸는 방법을 배워본다.

### 1. 변수와 상수

변수란 간단하게 이름이 붙은 값으로, 값은 언제든지 변할 수 있다는 것을 암시한다. 다음과 같이 사용한다.

```javascript
let currentTempc = 22; // 섭씨온도
```

위의 temp의 값은 언제든지 바꿔줄 수 있다.
위와 같이 꼭 초기값을 할당하지 않아도, 암시적으로 특별한 값 `undefined`가 할당된다.

```javascript
let targetTempC; // let targerTempC = undefined; 와 같다
```

다음과 같이 let문 하나에서 변수 여러개를 선언할 수 있다.

```javascript
let targetTempC,
  room1 = "conference_room_a",
  room2 = "lobby";
```

상수 `const`는 ES6에서 새로 생겼다. 한번 값을 할당하면 값을 변경할 수 없는 것이 특징

```javascript
const RoomTempC = 21.5;
```

절대적인 규칙은 아니지만, 상수 이름에는 보통 대문자와 밑줄만 사용한다.

### 2. 식별자 이름

변수와 상수, 함수의 이름을 식별자라고 한다. 그리고 식별자에 규칙이 있다.

- 식별자는 반드시 글자나 달러기호(\$), 밑줄(\_)로 시작해야한다.
- 식별자에는 글자와 숫자, 달러기호, 밑줄만 사용할 수 있다.
- 유니코드 문자도 사용가능
- 예약어는 식별자로 쓸 수 없다.
- 대문자로 시작해서는 안된다. 물론 예외는 있다.
- 밑줄 한개, 혹은 두개로 시작하는 식별자는 아주 특별한 상황, 또는 '내부'변수에서만 사용한다. 그러니 엥간하면 사용하지말자
- 제이쿼리를 사용할때, 달러 기호로 시작하는 식별자는 제이쿼리 객체라는 의미입니다.

### 3. 리터럴

리터럴은 값을 만드는 방법이다. 자바 스크립트는 내가 제공한 리터럴 값을 받아 데이터를 만든다.
리터럴과 식별자의 차이를 아는 것이 중요하다. 예를 들어 보자

```javascript
let room1 = "conferenceRoomA"; // conferenceRoomA는 문자열 리터럴.

let currentRoom = room1; // 이제 currentRoom의 값은 room1의 값("conferenceRoomA")와 같습니다.

currentRoom = conferenceRoomA; // error!
// conferenceRoomA란 식별자는 존재하지 않습니다.
```

어디에 변수를 쓰고 어디에 상수를 쓸지 결정하는 것은 프로그래머의 몫이다.

### 4. 원시타입과 객체

자바스크립트의 값은 원시 값 또는 객체이다. 문자열과 숫자같은 원시 타입은 불변(immutable)이다.

원시타입의 종류는 다음과 같다.

- 숫자
- 문자열
- 불리언
- null
- undefined
- 심볼

다만, 불변성이라는 말이 변수의 값이 바뀔 수 없다는 뜻은 아니다.

```javascript
let str = "hello";
str = "world";
```

위를 보면, `str`은 `"hello"`라는 값으로 초기화했고, 다시 새로운 불변값 `"world"`를 할당 받는다. 중요한 것은 `hello`와 `world`가 다른 문자열이라는 것이다. 바뀐 것은 `str`이 저장하는 값뿐.

이러한 원시타입외에 객체가 있다. 원시 값과 다르게 객체는 여러 가지의 형태와 값을 가질 수 있다.

객체는 유연한 성질 때문에 데이터 타입을 만들 때 객체를 많이 사용한다. 자바스크립트에는 다음과 같이 몇가지 내장된 객체가 존재한다.

- Array
- Date
- RegExp
- Map과 WeakMap
- Set과 WeakSet

마지막으로, 원시 타입 중 숫자와 문자열, 불리언에 각각 대응하는 객체 타입은 `Number`, `String`, `Boolean`이 있다. 이들 대응하는 객체에 실제 값이 저장되지는 않는다.

### 5. 숫자

자바 스크립트도 다른 프로그래밍 언어와 마찬가지로 실제 숫자의 근사치를 저장할 때 `IEEE-764 배정도 부동소수점 숫자 형식`을 사용한다. 이 방식은 `Double`이라고도 한다.

자바 스크립트는 10진수, 2진수, 8진수, 16진수의 4가지 숫자형 리터럴을 인식한다.

```javascript
let count = 10; // 10진수
const blue = 0x0000ff; // 16진수
const umask = 0o0022; // 8진수
const roomTemp = 21.5; // 십진수
const c = 3.0e6; // 지수
const e = -1.6e-19; // 지수
const inf = Infinity;
const ninf = -Infinity;
const nan = NaN; // 숫자가 아님
```

숫자에 대응하는 Number객체에는 중요한 숫자형 값에 해당하는 유용한 프로퍼티가 있다.

```javascript
const small = Number.EPSILON; // 1에 더했을 때, 1과 구분되는 결과를 만들 수 있는 가장 작은 값이다.
const bigInt = Number.MAX_SAFE_INTEGER; // 표현할 수 있는 가장 큰 정수
const max = Number.MAX_VALUE; // 표현할 수 있는 가장 큰 숫자
const minInt = Number.MIN_SAFE_INTEGER; // 표현할 수 있는 가장 작은 정수
const min = Number.MIN_VALUE; // 표현할 수 있는 가장 적은 숫자
const nInf = Number.NEGATIVE_INFINITY; // -Infinity
const nan = Number.NaN; // NaN
const inf = Number.POSITIVE_INFINITY; // Infinity
```

### 6. 문자열

자바스크립트의 문자열은 유니코드 텍스트이다.
자바스크립트의 문자열 리터럴에는 ',",`을 사용한다.

### 7. 이스케이프

텍스트로 만들어진 프로그램에서 텍스트 데이터를 사용할 땐, 항상 데이터와 프로그램 자체를 구분할 방법이 필요하다. 이때 문자열을 따옴표안에 쓰는 방법이 있으나, 만약 문자열 안에 따옴표를 써야할때는 말이 달라진다.

이런 문제를 해결하려면 따옴표를 이스케이프해서, 문자열 주위에 쓰는 부호가 아님을 나타내줘야한다.

예제를 통해 알아보자

```javascript
const dialog = 'Sam looked up, and said "hello, old friend", as Max walked in.';
const imperative = "Don't do that!";
```

위에서는 이스케이프가 필요하지 않다. `dialog`는 작은 따옴표로 감싸여 있으므로, 안에 큰 따옴표를 걱정없이 쓸 수 있기 때문. 하지만 두가지 따옴표를 모두 써야한다면?

```javascript
// error!
const dialog = "sam looked up and said "don't do that!" to max"
```

위처럼 쓰면 에러가 발생한다. 이때 역슬래시(\)를 사용해서 따옴표를 이스케이스 하면 문자열이 여기서 끝나지 않았다고 자바스크립트에게 알려줄 수 있다. 다음과 같이 고치면 된다.

```javascript
const dialog1 = 'He looked up and said "don\'t go that!" to Max';
const dialog2 = 'He looked up and said "don\'t do that!" to Max';
```

이제 여기서 떠오르는 문제. 문자열에서 역슬래시를 써야할 때는 어떻게 해야하지?
다행히도 역슬래시는 자기 자신을 이스케이프 할 수 있다.

```javascript
const s = "In Javascript, use \\ as en escape character in strings.";
```

큰 따옴표를 쓸 지, 작은 따옴표를 쓸지는 사용자에게 달려있다.
