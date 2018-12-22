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



