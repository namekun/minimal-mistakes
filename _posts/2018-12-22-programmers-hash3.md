---
title: "프로그래머스 코딩테스트 연습 문제 - Hash - 위장"
tags:
  - python
  - algorithm
  - hash
  - 프로그래머스
comments: true
---

### 문제 설명

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

### 입출력 예

| clothes0                                                     | return |
| ------------------------------------------------------------ | ------ |
| [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
| [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |

### 입출력 예 설명

예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

---

자 생각해보자. 이건 딕셔너리를 확실하게 이용해서 풀 수 있을것만 같다.

우리가 해결해야할 문제를 푸는 과정은 다음과 같다.

1. 딕셔너리를 만들어 주어진 이차원 배열의 값을 `부위-개수`로 저장한다. 굳이 이름으로 저장해줄 이유가 하나도 없다.

2. 옷 입어보는 방법의 수를 구하는 공식을 만들어보자.

   위의 예시를 보았을 때, 모자의 종류는 2개, 안경의 종류는 1개이다. 이때 안경+모자의 조합의 개수를 구하자면 `2 * 1`인 2개이다. 그리고 각자 한 개씩만 착용하는 경우의 수 3개. 총 가능한 경우의 수는 5개이다.

   한 개씩만 착용하는 경우의 수를 어떻게 구할까? 

   이때는 각 종류의 개수에 안입는 경우를 1개를 더해준다. 그러니까 다음처럼 된다.

   ```python
   [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear],[none, headgear], [none, eyewear]]
   ```

   이제 모자의 종류 개수 3개, 안경의 종류 개수 2개이다. 이 모든 경우의 수를 곱하면 6가지 방법이 나오고, 이 때 아무것도 안입는 `none * none`의 경우의 수인 1을 빼주면 된다.

   이는 종류가 3가지 이상이라도 통용이 된다. 이해가 안되면 공책에 적어가면서 해보자!

이제 위의 알고리즘을 코드로 나타내보자.

```python
def solution(clothes):
    answer = 1
    hash_map = dict()
    n = len(clothes)
	
    # make hash_map
    for i in range(n):
        if clothes[i][1] in hash_map:
            hash_map[clothes[i][1]] += 1
        else:
            hash_map[clothes[i][1]] = 1
	
    # operate
    for j in hash_map:
        answer *= (hash_map[j] + 1)
	
    # 아무것도 입지 않는 경우의 수 제거
    answer -= 1

    return answer
```

잘돌아간다.

방-긋

---

다른 사람들은 어떻게 풀었는지 궁금해서 보는데 볼 때마다 상상초월이다.

코드를 보자

```python
def solution(clothes):
    from collections import Counter
    from functools import reduce
    cnt = Counter([kind for name, kind in clothes])
    answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
    return answer
```

개인적으로는 이렇게 보는걸 익숙치 않아서 그런지 모르겠지만, 다양한 연산을 한줄로 줄이는 것을 그렇게 좋아하지않는다.

그렇지만 익숙해져야하고, 훨씬 이득이 되기에 위 풀이를 적용한 사람의 코드에 있는 모든 기술을 분석해보고 습득해야만 한다.

### Counter

Counter 모듈은 리스트에 있는 각 항목을 셀 수 있는 기능이다.

Counter는 다음과 같이 사용한다.

```python
from collections import Counter
```

리스트를 하나 주어주고, `Counter` 메서드를 사용해보자.

```python
l = [1,2,3,4,5,3,4,5,2,3,4,5,23,3,4,3,4,5,2,2,34]

Counter(l)

# result
# Counter({1: 1, 2: 4, 3: 5, 4: 5, 5: 4, 23: 1, 34: 1})
```

결과는 다음과 같이 나온다.

물론 `String`도 이렇게 사용가능하다.

```python
random  = 'sselirjalijrlaijrliawenrlinvlaidlivjawlijer'
Counter(random)

# Counter({'a': 5,
#          'd': 1,
#          'e': 3,
#          'i': 8,
#          'j': 5,
#          'l': 8,
#          'n': 2,
#          'r': 5,
#          's': 2,
#          'v': 2,
#          'w': 2})
```

### reduce

reduce는 요소 처음부터 순차적으로 지정된 함수로 처리해준다. 다음 예시는 리스트에 저장된 요소를 처음부터 하나씩 더한다.

```python
a = [1, 2, 3, 4, 5]
from functools import reduce
# 람다 표현식 맨 뒤에 사용할 리스트 a를 넣어준다.
reduce(lambda x, y : x + y , a) 
```

`lambda x, y: x + y`와 같이 매개변수 두 개를 받아서 더한 결과를 반환한다. 즉, 리스트 요소의 합을 구한다. 참고로 reduce는 파이썬 3부터 내장 함수가 아니다. 따라서 `from functools import reduce`와 같이 `functools` 모듈에서 `reduce` 함수를 가져와야만 한다.

그 이외로, 초기값도 넣어줄 수 있는데 초기값을 넣을때에는 사용할 리스트 뒤에 `, 초기값` 으로 넣어주면 된다.

다음 그림을 통해서 어떻게 연산되는지 이해를 높이자.

![](https://dojang.io/pluginfile.php/5358/mod_page/content/5/034004.png)

### 코드 분석

```python
def solution(clothes):
    from collections import Counter
    from functools import reduce
    cnt = Counter([kind for name, kind in clothes])
    answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
    return answer
```

코드를 한줄 한줄 분석해보자.

위의 `from~`은 그냥 모듈을 가져오는 코드이니 pass하고, 그 다음줄부터 보자.

```python
cnt = Counter([kind for name, kind in clothes])
```

`kind`라는 리스트에 `for`문으로 `(name, kind)`튜플 형식으로 데이터를 집어 넣는다. 그리고 그 데이터를 `Counter`함수로 개수를 세어준다.

```python
answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
```

다음 코드는 1이라는 초기값으로 시작해서 cnt의 값들을 하나하나 +1 을 해주고 곱셈해준다. 그 뒤에 아무것도 안입는 경우의 수 1을 빼준다.
