---
title: "모두의 알고리즘 with python - QuickSort"
tags:
  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# Quick Sort

퀵소트는 '그룹을 반으로 나눠서 재귀 호출'하는 방식은 병합 정렬과 다를 바 없지만, 그룹을 나눌 때 미리 기준과 비교해서 나눈다는 점이 다르다!

즉, 먼저 기준과 비교해서 그룹을 나누고, 다음 각각 재귀 호출해서 합치는 방식이라고 할 수 있다.

예시를 잘 못드니 쉬운 코드로 이해도를 높이도록 하자

*easyQuickSort.py*

```python
# easy quick sort
# input : list a
# output : sorted new List


def quickSort(a):
    n = len(a)
    # 종료 조건 : 정렬할 리스트의 자료 개수가 한 개 이하이면 정렬 X
    if n <= 1:
        return a

    # 기준 값을 정하고 기준에 맞춰 그룹을 나누는 과정
    pivot = a[-1]  # 기준은 자기 마음대로지만, 마지막 값으로 하기로 한다.
    g1 = []  # 기준보다 작은 값을 담을 리스트
    g2 = []  # 기준보다 큰 값을 담을 리스트

    for i in range(0, n - 1):
        if a[i] < pivot:
            g1.append(a[i])
        else:
            g2.append(a[i])

    # 각 그룹에 대해서 재귀 호출로 퀵 정렬을 하고
    # 기준 값과 합쳐 하나의 리스트로 결과값 변환

    return quickSort(g1) + [pivot] + quickSort(g2)  # 더러운데;아무튼 리스트를 이렇게 더할 수 있다.


d = [6, 8, 5, 9, 10, 1, 4, 3, 2, 7]

print(quickSort(d))
```



이해가 쉽다. 반나눠서 그안에서 기준정하고 또 나누고...의 반복일뿐.

그럼 이제 일반적인 퀵소트 알고리즘을 해보자.

위랑 다른 점은 입력 리스트 안에서 직접 위치를 바꾸며 정렬한다.

*normalQuickSort.py*

```python
# 리스트 a에서 어디부터 어디까지가 정렬 대상인지
# 범위를 지정하여 정렬하는 재귀 호출 함수

def quickSortSub(a, start, end):

    # 종료 조건
    if end - start <= 0:
        return

    # 기준값
    pivot = a[end]

    i = start

    # 정렬
    for j in range(start, end):
        if a[j] <= pivot:
            a[i], a[j] = a[j], a[i]
            i += 1
    
    a[i], a[end] = a[end], a[i]

    # 재귀 호출
    quickSortSub(a, start, i - 1)
    quickSortSub(a, i + 1, end)



def quickSort(a):
    quickSortSub(a, 0, len(a) - 1)


d = [6, 8, 5, 9, 10, 1, 4, 3, 2, 7]

quickSort(d)

print(d)
```

이 알고리즘도 계산 복잡도는 역시 `O(n*logn)`이다.

#### 연습문제

거품정렬의 구현이 연습문제다... 일단 해보자.

*BubbleSort.py*

```python
def bubbleSort(a):
    n = len(a)

    for i in range(n - 1):
        for j in range(n - i - 1):
            if a[j] > a[j + 1]:
                a[j], a[j + 1] = a[j + 1], a[j]


d = [6, 8, 4, 9, 10, 1, 2, 3, 7, 5]

bubbleSort(d)

print(d)
```

앞에서부터 하나하나 비교해가는 버블 정렬은 이렇게 for문이 도는 횟수를 주의해서 구현해주면 된다. 간-----단.
