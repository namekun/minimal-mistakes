---
title: "프로그래머스 코딩테스트 연습 문제 - Hash - 전화번호부 목록"
tags:
  - python
  - algorithm
  - hash
  - 프로그래머스
comments: true
---

### 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

* 구조대 : 119

* 박준영 : 97 674 223

* 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를. 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

phone_book의 길이는 1 이상 1,000,000 이하입니다.
각 전화번호의 길이는 1 이상 20 이하입니다.

### 입출력 예제

| phone_book                        | return |
| :-------------------------------- | ------ |
| ["119", "97674223", "1195524421"] | false  |
| ["123","456","789"]               | true   |
| ["12","123","1235","567","88"]    | false  |

### 입출력 예 

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

---

하 이건 어떻게 풀어야할까

처음 생각했던 풀이 방식은 문자열을 하나하나 비교해서 포함되어있는가? 라는 조건을 걸고 포함되어있다면 `answer = False`로 주는 방식이였다.

```python
def solution(phone_book):
    n = len(phone_book)
    phone_book = sorted(phone_book)

    for i in range(0, n - 1):
        for j in range(i + 1, n):
            if phone_book[i] in phone_book[j]:
                return False
    return True
```

여기서 사용한 `sorted`는 `sort`와 다르게 객체를 돌려준다. 그렇기에 앞에 `phone_book =`을 추가해 준 것.

물론 통과했다. 그렇지만 해시 문제를 해시로 풀지 않고, 중첩된 for문으로 풀었다는 것이 아주 찜찜해서 해시로 푸는 방법이 없는지 고민했다.

다음은 다른 사람이 풀은 코드를 참고하여 만든 해시를 이용한 풀이이다.

```python
def solutionUseHash(phone_book):
    n = len(phone_book)
    answer = True
    
	# 전화번호부를 길이에 맞춰서 정렬한다.
    phone_book = sorted(phone_book, key=lambda x: len(x))
    
    # 인덱스에 맞춰서 뽑아낸다
    for i, phone1 in enumerate(phone_book):
        hash_map = {}
        # 비교할 번호를 전화번호부에서 인덱스 다음부터 뽑아낸다.
        for phone2 in phone_book[i + 1:]:
            # 해시 맵에 비교할 번호를 기준 번호의 길이만큼 자른 뒤 해당 숫자를 키로, 값은 해당 번호로 저장한다.
            hash_map[phone2[:len(phone1)]] = phone2
        if phone1 in hash_map:
            answer = False
            break
    return answer
```

처음보는 개념들이 많다. 정리해보자.

### enumerate

`enumerate`는 "열거하다"라는 뜻이다. 이 함수는 순서가 있는 자료형(리스트, 튜플, 문자열)을 입력으로 받아 인덱스 값을 포함하는 enumerate 객체를 리턴한다.

보통 `for`문과 함께 사용한다. 다음 예시를 통해서 이해를 높이자

```python
for i , name in enumerate(['body', 'foo', 'bar']):
    print(i, name)
    
# 0 body
# 1 foo
# 2 bar
```

순서값과 함께 배열안의 원소들이 순서대로 출력되었다. 즉, 위의 예제와 같이 `enumerate`를 `for`과 함께 사용하면 자료형의 현재 순서와 그 값을 알 수 있다.

`for`문과 같이 반복되는 구간에서 객체가 현재 어느 위치에 있는지 알려주는 idx값이 필요하다면 `enumerate`함수를 사용하면 된다.

### Zip

`zip`함수는 보통 `list` 여러개를 slice할 때 사용한다. 우선 여러 개의 `list`를 하나의 묶음으로 묶고, 이를 잘라내버린다. 다음 예시를 보며 이해를 높이자.

```python
a = [1, 2, 3, 4, 5]
b = ['a','b','c','d','e']

for x, y in zip(a, b):
    print x,y

# 1 a
# 2 b
# 3 c
# 4 d
# 5 e
```

이런식으로 마치 김밥을 자르듯이 자르게 된다. `enumerate`랑은 비슷해보이지만, 예시만 그래서 그렇지 엄연히 다른 메서드이다.

### lambda

람다는 쉽게 말하자면 '이름이 없는 함수'이다. 함수의 이름을 작성하지 않고도 바로 사용할 수 있다는 의미이다. 보통 람다함수는 한 번 사용하고 마는 함수를 사용할 때 효과적이고, 덜 지저분해진다는 장점을 가질 수 있다.

보통 람다는 다음과 같이 사용한다.

```python
lambda x: 원하는 연산
```

### sorted

`sorted(iterable)` 함수는 입력값을 정렬한 후 그 결과를 리스트로 리턴하는 함수이다. 다음과 같이 사용한다.

```python
x = sorted(list, 정렬할 때 기준이 되는 key) 
```

---

또한 어떻게 풀어야할까 고민하다가 새로운 메서드를 발견했다!

바로 `startswith()` 메서드. 파이썬 `startswith()` 메서드는 문자열, 시작 부분에 서브 문자열을 지정 여부를 확인하는 데 사용된다. 

문법은 다음처럼 사용하면 된다.

```python
str.startswith(str, begin = 0, end = len(String))
```

* `str`은 문자열
* `begin`은 문자열 검색의 시작점
* `end`는 문자열 검색의 종료점

세상에..파이썬에는 아직 내가 모르는 메서드가 많다.... 갈 길이 멀구나

그럼 발견한 메서드를 이용해서 위의 문제를 해결하는 코드를 만들어보자.

```python
def solutionUseStartswith(phone_book):
    phone_book = sorted(phone_book)
	
    # 정렬했으니까 그냥 그대로 묶어서 사용한다.
    for p1, p2 in zip(phone_book, phone_book[1:]):
        if p2.startswith(p1):
            return False

    return True
```

이번 문제는 어째저째 해결하긴 했지만, 뭔가 아직 부족함이 많이 느껴졌던 순간이였다. 위의 문제를 해결하는 방법에는 위에서 제시했던 방법말고도, 정규표현식으로 푸는 방법도 있고 뭐 다양했다. 문제를 풀어가면서 하나하나 습득해가야겠다.
