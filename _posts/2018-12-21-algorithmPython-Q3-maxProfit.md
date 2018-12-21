---
title: "모두의 알고리즘 with python - 알고리즘 문제풀이 3 - Find Max Profit!"
tags:
  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# 최대 수익 알고리즘

어떤 주식에 대해 특정 기간 동안의 가격변화가 주어졌을 때, 그 주식 한 주를 한 번 사고팔아 얻을 수 있는 최대 수익을 계산하는 알고리즘을 만들어 보자.

어떤 주식이 아래의 표처럼 매일 변했다고 치자.

| 날짜 | 주가(원) |
| :--: | :------: |
| 6/1  |  10300   |
| 6/2  |   9600   |
| 6/3  |   9800   |
| 6/4  |   8200   |
| 6/5  |   7800   |
| 6/8  |   8300   |
| 6/9  |   9500   |
| 6/10 |   9800   |
| 6/11 |  10200   |
| 6/12 |   9500   |

이 주식 한 주를 한 번 사고팔아 얻을 수 있는 최대 수익은 얼마일까? 단, 손해가 나면 주식을 사고팔지 않아도 된다. 따라서 최대 수익은 항상 0이상의 값이다.

### 문제 분석과 모델링

주식 거래로 가장 많은 수익을 내는 방법은 가장 쌀 때 사서 가장 비쌀 때 파는 것이다. 그러나 주가의 최대값에서 최소값을 그냥 빼라는 것이 아니다. 최소 가장 비쌀 때는, 주식을 산 날짜보단 뒤여야한다.

이 문제를 어떻게 풀어야할까?

우선 가능한 모든 경우를 다 비교해보는 법이 있다. 가격을 전부다 비교해보는 것이다. 1, 2, 3일차 이렇게..

```python
# input : prices
# output : maxProfit

def maxProfit(prices):
    n = len(prices)
    maxPro = 0

    for i in range(0, n - 1):
        for j in range(i + 1, n):
            profit = prices[j] - prices[i]

            if profit > maxPro:
                maxPro = profit

    return maxPro


stock = [10300, 9600, 9800, 8200, 7800, 8300, 9500, 9800, 10200, 9500]
print(maxProfit(stock))
```

구현하는 법은 굉장히 쉽다. 그러나 너무 많은 불필요한 비교를 하는 점에 있어서 좋은 해결방법은 아니라고 할 수 있다. 그렇다면 훨씬 좋은 방법은 없을까?

이전에는 사는 날을 기준으로 해서 매일 비교를 했다면 이제는 파는 날을 기준으로 생각해보자. 파는 날을 기준으로 이전 날들의 주가 중에서 최소값만 알 수 있다면, 최대 수익을 쉽게 계산할 수 있다. 알고리즘을 말로 짜보면 다음과 같다.

1. 최대 수익을 저장하는 변수를 만들고 0을 저장한다.
2. 지금까지의 최저 주가를 저장하는 변수를 만들고, 첫째 날의 주가를 기록한다.
3. 둘째 날의 주가부터 마지막 날의 주가까지 비교한다.
4. 반복하는 동안 그날의 주가에서 최저 주가를 뺀 값이 현재 최대 수익보다 크면 최대 수익 값을 그 값으로 고친다.
5. 그날의 주가가 최저주가보다 낮으면 최저 주가의 값을 그날의 주가로 한다.
6. 처리할 날이 남았다면, 4번 과정으로 돌아가고, 다했다면 최대 수익에 저장된 값을 돌려주고 종료한다.

```python
def maxProfit(prices):
    n = len(prices)
    maxPro = 0
    minPrice = prices[0]

    for x in range(1, n):
        profit = prices[x] - minPrice
        if profit > maxPro:
            maxPro = profit

        if prices[x] < minPrice:
            minPrice = prices[x]

    return maxPro


stock = [10300, 9600, 9800, 8200, 7800, 8300, 9500, 9800, 10200, 9500]
print(maxProfit(stock))
```

당연히 시간 복잡도는 아래의 알고리즘이 `O(n)`으로 더 낮다.

직접 시간 복잡도를 비교해서 눈으로 얼마나 차이가 나는지 알아보자.

```python
# 최대 수익 문제를 푸는 두 알고리즘의 계산 속도 비교하기
# 걸린 시간을 출력/ 비교한다.

import time
import random

# 느린 알고리즘

def slowMaxProfit(prices):
    n = len(prices)
    maxPro = 0

    for i in range(0, n - 1):
        for j in range(i + 1, n):
            profit = prices[j] - prices[i]

            if profit > maxPro:
                maxPro = profit

    return maxPro

# 빠른 알고리즘
def speedMaxProfit(prices):
    n = len(prices)
    maxPro = 0
    minPrice = prices[0]

    for x in range(1, n):
        profit = prices[x] - minPrice
        if profit > maxPro:
            maxPro = profit

        if prices[x] < minPrice:
            minPrice = prices[x]

    return maxPro

def test(n):
    # 난수로 테스트 자료 만들기

    a = []
    for i in range(0, n):
        a.append(random.randint(5000, 20000))

    # 느린 알고리즘 테스트
    start = time.time()
    mps = slowMaxProfit(a)
    end = time.time()
    time_slow = end - start

    # 빠른 알고리즘 테스트
    start = time.time()
    mpf = speedMaxProfit(a)
    end = time.time()
    time_fast = end - start

    # 결과 출력
    print(n, mps, mpf)

    # 결과 출력 : 계산 결과 비교
    m = 0 # 느린 알고리즘과 빠른 알고리즘의 수행 시간 비율을 저장할 변수
    if time_fast > 0: # 컴퓨터 환경에 따라 빠른 알고리즘 시간이 0으로 측정될 수 있음
        # 이럴때는 0을 출력
        m = time_slow / time_fast
    # 입력크기, 느린 알고리즘 수행시간, 빠른 알고리즘 수행시간, 계산 시간 차이
    # %d는 정수 출력,  %.5f 는 소수점 다섯 자리까지의 출력을 의미
    print("%d %.5f %.5f %.2f" % (n, time_slow, time_fast, m))


test(100)
test(10000)
```



결과는 다음과 같다.(입력크기, 최대 수익, 느린알고리즘 수행시간, 빠른 알고리즘 수행시간, 느린 / 빠른)

| 입력크기 | 최대 수익 | 느린 알고리즘 수행시간 | 빠른 알고리즘 수행시간 | 느린 / 빠른 |
| -------- | --------- | ---------------------- | ---------------------- | ----------- |
| 100      | 14506     | 0.00100                | 0.00000                | 0.00        |
| 10000    | 14998     | 6.38332                | 0.00100                | 6380.74     |

입력 크기가 100 일때는 그렇게 수행시간의 차이가 크지 않았는데, 입려 크기가 10000으로 되니 엄청나게 차이가 나기 시작했다.

이것으로 `O(n)`과 `O(n^2)`가 얼마나 큰 차이가 나는지 알 수 있었다.

이것으로 책을 마친다.
