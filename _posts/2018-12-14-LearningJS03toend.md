---
title: "LearningJS-CH03-3.8부터 끝까지"
tags:
  - Javascript
  - learningJS
comments: true
---

### 3.8 특수문자

역슬래시는 단순하게 이스케이프 할 때만 사용하는 것이 아니다. 줄바꿈 문자처럼 화면에 표시되지 않는 일부 특수 문자나, 임의의 유니코드 문자를 나타낼때에도 사용한다. 아래는 주로 사용되는 특수문자이다.

| 코드 | 설명         | 예제             |
| ---- | ------------ | ---------------- |
| \n   | 줄바꿈 문자. | "line1\line2"    |
| \r   | 캐리지 리턴  | "line1\r\nline2" |
| \t   | Tab          | "Speed:\t60kph"  |

그 이외의 것은 생략.

### 3.8.1 템플릿 문자열

값을 문자열 안에 써야하는 일이 있을 때, 문자열 병합을 통해 변수나 상수를 문자열 안에 사용할 수 있다.

```javascript
let currentTemp = 19.5;
//00b0은 온도를 나타내는 유니코드
const message = "The current temperature is " + currentTemp + "\u00b0c"

// result 
// The current temperature is 19.5°c
```

ES6 이전엔 위처럼 변수나 상수를 문자열 안에 쓰는 방법은 문자열 병합뿐이였다. 그러나 ES6에서 문자열 템플릿이라는 기능을 도입했고, 이제 이를 통해서 위의 코드를 나타내보자.

```javascript
let currentTemp = 19.5;
//00b0은 온도를 나타내는 유니코드
const message = `The current temperature is ${currentTemp}\u00b0c`

// result 
// The current temperature is 19.5°c
```

다른 점이 있다면 `"`대신에 백틱(\`) 을 써준다는 것. 그리고 변수가 들어갈 자리는`${}`로 감싸준다는 것.

### 3.8.2 여러 줄 문자열

직접 코드를 통해 알아본다.

```javascript
const multiline = "line1\
line2";
```

위와 같은 코드가 있을 때,  multiline 문자열에 줄바꿈 문자는 들어가지 않는다. 첫행의 마지막 역슬래시는 줄바꿈 문자를 이스케이프하지만, 문자열에 줄바꿈 문자를 삽입하지는 않는다. 따라서 결과는 `line1line2`라고 나오게 된다. 이를 줄바꿈 문자가 들어가게 하려면 다음과 같이 써야한다.

```javascript
const multiline = "line1\n\
line2";

// result
//line1
//line2
```

백틱을 사용한 문자열에서는 조금 더 상식적인 결과를 볼 수 있다.

```javascript
const multiline = `line1
line2`;

// result
//line1
//line2
```

위의 코드의 결과에는 줄바꿈 문자가 들어가있다. 하지만, 어느쪽이던 상관없이 다음 줄 앞에 있는 들여쓰기가 결과 문자열에 포함되게 된다. 다음 코드를 보자

```javascript
const multiline = `line1
	line2
	line3`

//result
//line1
//	line2
//	line3
```

이렇게 했을때 결과값에 줄바꿈은 확실하게 들어가지만, 두 번째, 세 번째 줄에는 원하지않는 공백이 앞에 들어간다.

고로! 여러 줄 문자열을 사용하는 것은 권하는 방법이 아니다. 여러 줄 문자열을 쓰려면 코드를 읽기 쉽게 들여쓰기를 하던, 아니면 원치않는 공백을 넣던 둘 중 하나뿐이다. 보통 여러줄 문자열을 써야할때는 문자열 병합을 사용하도록 하자..

```javascript
const multiline = "line1\n"+
      "line2\n"+
      "line3";
```

문자열 병합을 사용할 때는 따옴표와 벡틱을 섞어써도 괜찮다.

### 3.8.3 숫자와 문자열

자바스크립트에서는 따옴표에 숫자를 넣어도 이게 숫자로 변하는 일이 있고, 그 반대로 되는 경우도 있다. 다음 예제를 보자

```javascript
const result1 = 3 + '30'; // 3이 문자열로 바뀐다. 결과값은 문자열 '330'
const result2 = 3 * '30'; // 30이 숫자열로 바뀐다. 결과값은 숫자 90
```



혼돈스럽다.... 그냥 숫자일땐 숫자를 씁시다.

### 3.9 불리언 생략

### 3.10 심볼

심볼은 유일한 토큰을 나타내기 위해서 ES6에서 도입한 새 데이터 타입이다. 심볼은 항상 '유일'하다. 다른 어떤 심볼과도 일치하지 않는다. 이런 면에서 심볼은 객체와 유사하다고 할 수 있다. 

객체는 모두 유일하다. 항상 유일하다는 점을 제외한다면 심볼은 원시 값의 특징을 모두 갖고있기에 확장성 있는 코드를 만들 수 있다.

심볼은 다음과 같이 `symbol()`생성자로 만든다. 원한다면 간단한 설명도 붙여줄 수 있다. 예제 코드를 보자.

```javascript
const RED = Symbol("The color of a sunset!");
const ORANGE = Symbol("The color of a sunset!");
RED === ORANGE // false. 심볼은 모두 다르기 때문.
```

고유한 식별자가 필요하다면, 심볼을 사용하도록 하자.



### 3.11 null & undefined

`null`과 `undefined`는 모두 존재하지 않는 것을 나타내나, 이를 어떻게 구분해야하는지는... 혼란스럽다. 

고로 우리는 이렇게 기억하도록 하자.

`null`은 프로그래머에게 허용된 데이터 타입이고, `undefined`는 자바스크립트 자체에서 사용한다고 기억하자.

잘 모르겠다면 그냥 `null`을 사용하는것이 마음에 개비스콘이다..



### 3.12 객체

원시 타입과 달리, 객체는 여러 가지 값이나 복잡한 값을 나타낼 수 있고, 변할 수도 있다. 객체의 본질은 '컨테이너'이다. 내용물은 시간이 지나며 바뀔 수 있으나, 내용물이 바뀐다고 컨테이너가 바뀌는 것은 아니다. 즉, 여전히 같은 객체라고 할 수 있다.

객체에도 중괄호, 즉 `{}`를 사용하는 리터럴 문법이 있다. 중괄호를 통해서 객체가 어디서 시작했고, 어디서 끝나는지 알 수 있다. 

코드로 예시를 들어보자

```javascript
const obj = {};
```

객체의 컨텐츠는 프로퍼티 또는 멤버라고 한다. 이 중 프로퍼티는 이름(키)과 값으로 구성된다. 프로퍼티의 이름은 반드시 문자열 또는 심볼이어야하며, 값은 어떤 타입이든 상관없고, 다른 객체여도 된다. 이제 저 위의 `obj`에 프로퍼티를 추가해보자.

```javascript
obj.color = "yellow";
```

프로퍼티 이름에 유효한 식별자를 써야 멤버 접근 연산자(`.`)를 사용할 수 있다. 프로퍼티 이름에 유효한 식별자가 아닌 이름을 쓴다면 계산된 멤버 접근 연산자(`[]`)를 써야한다. 프로퍼티 이름이 유효한 식별자여도 대괄호로 접근할 수 있다.

```javascript
obj["not an identifier"] = 3;
obj["not an identifier"]; // 3
obj["color"] // "yellow"
```

심볼 프로퍼티에 접근할 때에도, 대괄호를 사용한다.

```javascript
const SIZE = Symbol();
obj[SIZE] = 8;
obj[SIZE]; // 8
```

이제 `obj`에는 `"color"(유효한 식별자 문자열)`, `"not an identifier"(유효한 식별자가 아닌 문자열)`, `SIZE(심볼)` 세 가지의 프로퍼티가 있다.

`console.log(obj)`를 해보면, `{ color: 'yellow', 'not an identifier': 3, [Symbol()]: 8 }`라고 나오는 것을 볼 수 있다. 여기서 심볼 프로퍼티는 다른 것들과 다르게 처리되며, 기본적으로는 표시가 안된다. 해당 프로퍼티의 키는 `SIZE` 심볼이며, 문자열 `"SIZE"`가 아니기 때문이다.

우리는 `obj`객체를 빈 객체로 만들어서 시작했지만, 객체 리터럴 문법에서는 객체를 만드는 동시에 프로퍼티를 만들 수 있다. 중괄호 안에서 각 프로퍼티를 쉼표로 구분하고, 프로퍼티 이름과 값은 콜론으로 구분한다.

```javascript
const sam1 = {
    name: 'Sam',
    age: 4,
};

const sam2 = {name: 'Sam', age: 4}; // 한 줄로 선언한다.

const sam3 = {	// 프로퍼티 값도 객체가 될 수 있다.
    name: 'Sam',
    classification: {
        kingdom : 'Animalia',
        phylum: 'Chordata',
        class: 'Mamalia',
        order: 'Carnivoria'
    }
}
```

위에서는 객체를 3개 만들었다. `sam1`과 `sam2`의 프로퍼티는 같지만, 다른 객체이다. `sam3`에서 kingdom에 접근한느 방법은 여러가지이다. 여기서는 큰 따옴표만 썼지만, 이는 작은 따옴표, 백틱을 사용해도 된다.

```javascript
sam3.classfication.kingdom; // "Animalia"
sam3["classfication"].kingdom;
sam3.classfication["kingdom"];
sam3["classfication"]["kingdom"];
```

객체를 함수에도 담을 수가 있다. 

```javascript
sam3.speak = function(){return "Meow!";};
// 함수 호출
sam3.speak(); // "Meow!"
```

마지막으로 객체를 제거할때는, delete연산자를 사용한다.

```javascript
delete sam3.classfication; // classification 트리 전체가 삭제
delete sam3.speak; // speak 함수가 삭제되었다.
```

### 3.13 Number, String, Boolean 객체 - 생략

### 3.14 배열

자바스크립트의 배열은 특수한 객체이다. 일반적인 객체와 다르게 배열 컨텐츠에는 항상 순서가 잇고, 키는 순차적인 숫자이다.

다음은 자바스크립트 배열의 특징이다.

* 배열 크기는 고정되지않는다. 언제든 요소를 추가하거나 제거할 수 있다.
* 요소의 데이터 타입을 가리지 않는다. 즉, 문자열만 이라거나 숫자만 쓸 수 있는 배열의 개념이 없다.
* 배열 요소는 0으로 시작한다.
* 배열 리터럴은 대괄호 안에 배열 요소를 쉼표로 구분해서 쓴다.

```javascript
const a1 = [1, 2, 3, 4]; // 숫자로 구성된 배열
const a2 = [1, 'two', 3, null]; // 여러가지 타입으로 구성된 배열
const a3 = [
    {name: "Ruby", hardness: 9},
    {name: "Diamond", hardness: 10},
]; // 객체가 들어있는 배열
const a4 = [ 
    [1, 4, 5],
    [3, 7, 8],
]; // 배열이 들어있는 배열
```

그 외 배열의 특징적인 기능은 거의 대부분 타언어와 비슷하다.

`length`라던가, 인덱스 접근이라던가..



### 3.15 객체와 배열 마지막의 쉼표

위에 코드를 봐오면서 이상한 점이 없는가? 그렇다면 눈이..

배열의 마지막 요소에 계속 `,`가 사용되어왔다. 왜 이렇게 하는가?

뭐 안해도 상관은 없다. 그냥 이 책의 저자가 하는 것...



### 3.16 날짜

자바스크립트의 날짜와 시간은 내장된 `Date` 객체에서 담당한다.

날짜를 나타내는 객체를 만들 때는 `new Date()`를 사용한다.

```javascript
const now = new Date();
now; // 현재 시간, 날짜등등..
```

특정 날짜에 해당하는 객체를 만들 때는 다음과 같이해둔다.

```javascript
const halloween = new Date(2018, 9, 31, 19, 0);
// 월은 0에서 시작한다. 즉, 9는 10월
// 시간은 19:00을 나타낸것. 
```

날짜 객체를 통해서 다음과 같은 부분도 가져올 수 있다.

```javascript
const day1 = new Date();

day1.getFullYear();
day1.getMonth();
day1.getDate();
day1.getHours();
day1.getDay(); // 1은 월요일. 0은 일요일
```



### 3.17 정규표현식 - 17장에서 봅시다.

### 3.18 맵과 셋 - 10장에서 봅시다.

### 3.19 데이터 타입 변환

데이터 변환하는 방법을 알아보자

1. 숫자로 바꾸기

   숫자로 바꾸는 방법에는 여러가지가 있다.

   첫번째는 Number객체 생성자를 사용하는 방법이다.

   ```javascript
   const numStr = "33.3";
   const num = Number(numStr); // 이 행은 숫자를 만든다.
   						 // Number 객체의 인스턴스가 아니다.
   ```

   숫자로 바꿀 수 없는 경우에는 `NaN`이 반환된다.

   두번째는 `parseInt` 또는 `parseFloat`함수를 사용하는 방법이다. 그 중 `parseInt`를 사용할 때는 기수(radix)를 넘길 수 있다. 기수는 변환할 문자열이 몇 진수 표현인지 지정한다.

   기수 기본 값은 10이므로 10진수 표현을 변환할 때에는 상관없으나, 항상 기수를 표시하는 것이 좋다.

   ```javascript
   const a = parseInt("16 volts", 10); // "volts"는 무시되고, 10진수 16이 나온다.
   const b = parseInt("3a", 16); // 16진수 3a를 10진수로 바꾼다. 결과는 58.
   const c = parseFloat("15.5 kph"); // "kph"는 무시된다. 
   							   // parseFloat는 항상 기수가 10이라고 가정된다.
   ```

   `Date`객체를 바꿀 때에는 `valueof`함수를 사용한다.

   ```javascript
   const d = new Date();
   const  ts = d.valueOf();
   // 1544719922310
   // 이는 UTC 1970년 1월 1일 자정으로부터 몇 밀리초가 지났는지 알려준다.
   ```

   아이고 의미없다.

   불리언 값을 1(true)나 0(false)로 바꿔야 할 때도 있다. 이때는 조건 연산자를 사용한다.

   ```javascript
   const b = true;
   const n = b? 1 : 0;
   ```

2. 문자열로 변환

   자바스크립트에도 `toString()`메서드가 있다! 알고있으니 써보면된다.

   ```javascript
   const n = 33.6;
   n; // 33.6 - 숫자
   const s = n.toString();
   s; // "33.6" - 문자열
   const arr = [1, true, "hello"];
   arr.toString(); // "1,true,hello" 로 변환
   ```

3. 불리언으로 변환

   부정 연산자(!)를 사용해서 모든 값을 불리언으로 바꿀 수 있다. 부정 연산자를 한 번 사용하면 '참 같은 값'은 `false`로 바뀐다. 한번 더 사용하면 `true`도 얻을 수 있다.

   ```javascript
   const n = 0; // 거짓 같은 값
   const b1 = !!n; // false
   const b2 = Boolean(n); // false
   ```



### 추가 . 참조형과 원시형

원시값은 불변이고, 원시값을 복사/전달할 때는 값 자체를 복사/전달한다. 따라서 '원본'의 값이 바뀌더라도 '사본'의 값이 따라서 변하지 않는다.

```javascript
let a = 1;
let b = a;
a = 2; // 원본의 값을 변화시킴
console.log(b) // 1. 사본의 값은 변하지 않는다.
```

또한 값 자체를 복사했기에 변수와 값은 일치한다.

```javascript
a === 2 // true
```

값 자체를 전달하므로 함수 안에서 변수의 값이 바뀌어도 함수 외부에서는 바뀌지 않은 상태로 남는다.

```javascript
function change(a){
    a = 5;
}

a = 3;
change(a);
console.log(a); // 3
```

객체는 가변이고, 객체를 복사/전달할 때는 객체가 아니라 그 객체를 가리키고 있다는 사실을 복사/전달한다. 따라서 원본이 바뀌면, 사본도 바뀐다! 와우! 이런 특징을 강조하고 싶을 때, 객체를 참조 타입이라고 부르기도 한다.

```javascript
let o = {a: 1};
let p = o; // 이제 p는 o가 '가리키고 있는 것'을 가리킨다.
o.a = 2
console.log(p); // {a:2}
```

주의해야할 점이 있다. 아래의 코드를 보자.

```javascript
let o = {a:1};
let p = o;
	p === o; // true
o = {a:2};	// 이제 o는 다른 것을 가리킨다. {a:1}을 수정한 것이 아니다.
	p === o // false
console.log(p); // {a:1}
```

객체를 가리키는 변수는 그 객체를 가리킬 뿐, 객체 자체는 아니다. 따라서 변수와 객체는 결코 일치하지 않는다.

```javascript
let q = {a:1};
q === {a:1}; // false
```

참조를 전달하므로 함수 안에서 객체를 변경하면 함수 외부에서도 바뀐다.

```javascript
function change_o(o){
    o.a = 999;
}

let o = {a:1};
change_o(o);
console.log(o); // {a:999}
```

