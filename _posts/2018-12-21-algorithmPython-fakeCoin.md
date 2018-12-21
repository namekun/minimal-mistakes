---
title: "모두의 알고리즘 with python - 알고리즘 문제풀이 2- Find fake coin!"
tags:
  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# 가짜 동전 찾기 알고리즘

겉보기에는 똑같은 동전이 n개가 있다. 이를 양팔 저울을 통해 무게를 비교하여 가짜동전을 찾아내고자 한다. 진짜보다 가벼운 가짜 동전을 찾아내는 알고리즘을 작성해보자

0번 부터 n - 1번째 동전까지 동전별로 우선 순서를 매긴다.

그리고 '저울질' 동작에 해당하는 함수를 우선 하나 만든다.

```python
def weigh(a, b, c, d):
```

이 함수는 동전의 a에서 b번까지, c에서 d번까지 나누고, 이 두 동전들을 양 팔에 올리고 무게를 비교하는 함수이다. 

해당 함수 안에 fake의 위치를 만들고, 그 값을 저장한다. 우리가 짜야할 가짜 동전 찾기 알고리즘은 이 가짜 동전의 위치를 알 수 없고, `weigh`함수를 호출해서 알아내야만 한다.

`weigh`함수의 결과값은 단 3가지이다. -1, 0, 1. -1이면 a-b 동전이 더 가볍다는 의미, 0이면 무게가 같기에 가짜동전이 없다는 의미, 1이면 c-d가 더 가볍다는 의미이다.

바로 이분 탐색이 생각났다. 그걸 이용해서 풀어보자.

```python
# 가짜 동전 찾기 알고리즘

def weigh(a, b, c, d):
    fake = 29
    if a <= fake and b >= fake:
        return -1
    elif c <= fake and d >= fake:
        return 1
    else:
        return 0


def findFake(left, right):
    # 만약 오른쪽과 왼쪽의 위치가 같아진다면, 가짜동전의 위치를 찾은 것이니 return 해준다.
    if left == right:
        return left

    half = (right - left + 1) // 2  # 0부터 n-1번째까지라서 + 1 해줘야한다.

    g1_left = left
    g1_right = left + half - 1
    g2_left = left + half
    g2_right = g2_left + half - 1

    result = weigh(g1_left, g1_right, g2_left, g2_right)

    if result == 1:
        return findFake(g2_left, g2_right)
    elif result == -1:
        return findFake(g1_left, g1_right)
    else:  # 두 집단의 무게가 같다면?
        return right  # 두 그룹으로 나뉘지않고, 마지막 한 개 남은 동전이 가짜동전이다.


n = 100
print(findFake(0, n - 1)) # 29
```

이분 탐색 이외에도, 순차 탐색을 이용해서 문제를 푸는 방법도 있다. 다음은 순차 탐색을 통해 문제를 푼 경우이다.

```python
# 가짜 동전 찾기 순차탐색 응용버전

def weigh(a, b, c, d):
    fake = 29
    if a <= fake and b >= fake:
        return -1
    elif c <= fake and d >= fake:
        return 1
    else:
        return 0


def findFake(left, right):
    for i in range(left + 1, right + 1):
        # 가장 왼쪽 동전과 나머지 동전을 차례대로 비교
        result = weigh(left, left, i, i)
        if result == -1:  # left 동전이 가벼움
            return left
        elif result == 1:  # 나머지 동전이 가벼움
            return i
        # 두 동전의 무게가 같다면 다음 동전으로

    # 모든 동전의 무게가 같다면, 가짜 동전이 없는 예외 상황
    return -1


n = 100
print(findFake(0, n - 1))
```

코드는 좀 더 간단하지만, 시간 복잡도가 `O(n)`이라는 점에서 위의 코드보다 시간 복잡도가 높다. 위의 코드는 `O(logn)`이다.
