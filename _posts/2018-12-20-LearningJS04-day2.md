---
title: "LearningJS-CH04.1-제어문의 기초 "
tags:
  - Javascript
  - learningJS
comments: true
---

## ch4.2 자바스크립트의 제어문

이제 제어문이 어떤 일을 하는지 확실히 알았고, 가장 기본적인 것들을 사용해 보았다. 이제 자바스크립트의 제어문에 대해 좀 더 알아보자.

### 1 . 제어문의 예외

`break` : 루프 중간을 빠져나간다.

`continue`: 루프에서 다음 단계로 바로 건너뛴다.

`return`: 제어문을 무시하고 현재 함수를 즉시 빠져나간다.

`throw`: 예외 핸들러에서 반드시 처리해야 할 예외를 일으킵니다. 예외 핸들러 현재 제어문 바깥에 있어도 상관없다.

### 2. if-else 문을 체인으로 연결하기

그 전에 만든 게임의 주인공이 수요일에는 운이 나쁘다는 미신을 갖고있고, 이 때는 딱 1펜스만 건다고 하면, 다음과 같이 `if-else` 체인으로 나타낼 수 있습니다.

```javascript
if(new Date().getDay() === 3){ // new Date().getDay()는 현재 요일에 해당하는 숫자를 반환한다. 0은 일요일이다.
    totalBet = 1;
} else if(funds === 7){
    totalBet = funds;
} else {
    console.log("No superstition here!");
}
```

이렇게 `if-else` 문을 이용한다면, 여러 조건을 걸고 그 중 하나를 선택하게 할 수 있다. 

### 3. 메타문법

메타 문법은 다른 문법을 설명하는 문법이다. 이번에 우리는 메타 문법을 사용해서 자바스크립트 제어문의 문법을 간결하게 표현해본다. 

여기서 기억해야할 것은 단 두 가지다. 대괄호로 감싼 것은 옵션이고, 생략 부호`...`는 여기 들어갈 내용이 더 있다. 라는 의미이다.

* while

```javascript
while(condition)
    statement
```

* if-else

```javascript
if(condition)
    statement1
[else
	statement2]
```

* do-while

```javascript
do
    statement
while(condition);
```

* for

```javascript
for([initialization]; [condition]; [final-expression])
    statement
```



### 4. for문의 다른 패턴

쉼표 연산자를 사용하면 초기화와 마지막 표현식에 여러 문을 결합할 수 있다. 예를 들어 다음의 `for`문은 피보나치 수열의 숫자 중 처음 여덞 개를 출력한다.

```javascript
for(let temp, i = 0, j = 1, j < 30, temp = i, i = j, j = i + temp)
    console.log(j);
```

이 예제에서 초기화를 하며 i, j, temp를 동시에 선언했고, 마지막 표현식에서 세 변수를 동시에 조작했다. `for` 루프의 제어부에 아무것도 쓰지 않는다면, 무한 루프가 만들어진다.

```javascript
for(;;) console.log("Infinity")
```

`for` 루프에서 조건을 생략하면 항상 true로 판단되기에 루프를 빠져나갈 수 없다.

`for`루프는 보통 정수 인덱스를 늘이거나 줄이면서 반복하지만, 꼭 그래야 하는 것은 아니다. 어떤 표현식이든 쓸 수 있기 때문! 다음 예제를 보자

```javascript
let s = '3'; // 숫자가 들어가 있는 문자열
for(; s.length<10; s = '' + s); // 문자열의 길이를 조건으로 사용
							 // 여기서 사용한 for 루프 마지막에 세미콜론이 없다면, 에러가 발생한다.

for(let x = 0.2; x<3.0; x+= 0.2) // 제어 변수가 정수가 아니여도 ok!

for(; !player.isBroke;) // 조건에 객체 프로퍼티를 쓴다.
    console.log("Still playing");
```

`for`문은 모두 `while`문으로 고쳐서 사용할 수 있다.

```javascript
for([initialization]; [condition]; [final-expression])
    statement
    
// 위의 for을 while로
[initialization]
while([condition]){
    statement
    [final-expression]
}
```



### 5. switch문

좀 더 다양한 조건을 두고 선택해야할 때 사용한다

```javascript
switch(expression){
    case value1:
        // expression을 평가한 결과가 value1일 때 실행된다.
        [break;]
    ...
    case valueN:
        // expression을 평가한 결과가 valueN일 때 실행된다
        [break;]
    default:
        // expression을 평가한 결과에 맞는 값이 없다면 실행된다.
        [break;]
}
```

아마 동적디스패치에 대해 배운다면 우리는 이걸 사용하지 않게 될 것이다.

### 6. for-in 루프

`for-in`루프는 객체의 프로퍼티에 루프를 실행하도록 설계된 루프이다. 문법은 다음과 같다.

```javascript
for(variable in obj)
    statement
```

다음 예제를 보자

```javascript
const player = {name: 'Francis', rank: 'FreshMan', age :"27"};
for(let prop in player){
    if(!player.hasOwnProperty(prop)) continue;
    console.log(prop + ":" + player[prop]);
}

// result
//name:Francis
//rank:FreshMan
//age:27
```

### 7. for-of 루프

`for-of` 문은 ES6에서 새로 생긴 반복문이며, 컬렉션의 요소에 루프를 실행하는 다른 방법이다. 문법은 다음과 같다.

```javascript
for(variable of obj)
    statement
```

`for-of`루프는 배열은 물론, 이터러블`iterable` 객체에 모두 사용할 수 있는 범용적인 루프이다. 다음 예제에서는 배열에 루프를 사용했다.

```javascript
const hand = [randFace(), randFace(), randFace()];
for(let face of hand)
    console.log(`You rolled ... ${face}!`);
```

`for-of`는 배열에 루프를 실행해야 하지만, 각 요소의 인덱스를 알 필요는 없을 때 알맞다. 인덱스를 알아야한다면, 일반적인 `for`루프를 사용하는 것이 더 낫다.

```javascript
const hand = [randFace(), randFace(), randFace()];
for(let i = 0; i < hand.length; i++)
    console.log(`Roll ${i+1}: ${hand[i]}`);
```

