---
title: "모두의 알고리즘 with python - BinarySearch"
tags:

  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

#  CH12  BinarySearch

이분 탐색은 둘로 나눈다는 뜻이다. 탐색할 자료를 둘로 나누어 찾는 값이 있을 법한 곳만 탐색하기 때문에 자료를 하나하나 찾아야 하는 순차 탐색보다 원하는 자료를 훨씬 빨리 찾을 수 있다.

빠르게 구현해보자

```python
# 리스트에서 특정 숫자 위치 찾기(이분 탐색)
# input : list a, find Val X
# output : 찾으면 그 값의 위치, 찾지 못하면 -1

def binarySearch(a, x):
    # 탐색할 범위를 저장하는 변수 start, end
    # 리스트 전체를 범위로 탐색 시작(0 ~ len(a) - 1)

    start = 0
    end = len(a) - 1

    while start <= end:
        mid = (start + end) // 2 # 탐색의 중간 위치
        if x == a[mid]:
            # Find!
            return mid
        # 아니라면 조건에 따라 반으로 나눠서 계속 탐색
        elif x > a[mid]:
            start = mid + 1
        else:
            end = mid - 1

    return -1

d = [1, 4, 9, 16, 25, 36, 49, 64, 81]

print(binarySearch(d, 36))
```

### 알고리즘 분석

이분 탐색은 값을 비교할 때마다 찾는 값이 잇을 범위를 절반씩 좁히면서 탐색하는 효율적인 탐색 알고리즘이라고 할 수 있다.

해당 알고리즘의 계산 복잡도는 `O(logn)`으로, 순차 탐색의 계산복잡도인 `O(n)`보다 훨~씬 효율적이라고 할 수 있다.

### 연습문제

다음 과정을 참고해서 재귀 호출을 이용한 이분 탐색 알고리즘을 만들어보자!

1. 주어진 탐색 대상이 비어있다면 탐색할 필요가 없다.(종료조건)
2. 찾는 값과 주어진 탐색 대상의 중간 위치 값을 비교한다.
3. 찾는 값과 중간 위치 값이 같다면, 결과값으로 중간 위치값을 돌려준다.
4. 찾는 값이 중간 위치값보다 크다면, 중간 위치의 오른쪽을 대상으로 이분 탐색 함수를 재귀 호출한다.
5. 찾는 값이 중간 위치값보다 착다면 중간위치의 왼쪽을 대상으로 이분 탐색함수를 재귀 호출한다.

```python
# Use Recursion BinarySearch


def binarySearch(a, start, end, x):
    # 종료 조건
    # 탐색 대상이 비어 있다면 탐색할 필요 없다.
    if a == []:
        return None

    mid = (start + end) // 2

    if x == a[mid]:
        return mid
    elif x > a[mid]:
        start = mid + 1
    else:
        end = mid - 1

    return binarySearch(a, start, end, x)


if __name__ == '__main__':
    d = [1, 4, 9, 16, 25, 36, 49, 64, 81]
    print(binarySearch(d, 0, len(d) - 1, 36))
```

