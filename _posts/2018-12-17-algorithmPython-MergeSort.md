---
title: "모두의 알고리즘 with python - mergeSort"
tags:
  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# 20181217 복귀

대림 최종면접을 보고 한동안 쉬다가 다시 시작합니다..

결과는 언제나오려나

## ch10 병합정렬

드디어 우리는 그전에 배웠던 재귀 호출을 이용해서 정렬을 해볼겁니다요.

병합정렬은 정렬해야할 집합을 반으로 나눕니다. 그 뒤에 반으로 나눈 것들끼리 정렬을 해주고, 각각 정렬된 두 집합을 서로 비교해가며 차례대로 하나씩 빼서 비교해가면서 작은 것 순으로 세우면 된다. 

뭔소린지 모르겟으면 코드로 이해하는게 빠르다.

자 쉽게 병합정렬을 코드로 만들어보자

*easyMergeSort.py*

```python
# easy merge sort
# input : list a
# output : sorted new List

def mergeSort(a):
    n = len(a)
    # end condition
    if n <= 1:
        return a
    # 그룹을 나누어 각각 병합 정렬을 호출하는 과정
    mid = n // 2  # 중간을 기준으로 두 그룹으로 나눈다.
    g1 = mergeSort(a[:mid])  # 재귀 호출로 첫 번째 그룹을 정렬
    g2 = mergeSort(a[mid:])  # 재귀 호출로 두 번째 그룹을 정렬

    # 두 그룹을 하나로 병합
    result = []  # 병합할 리스트

    while g1 and g2:  # 두 그룹에 모두 자료가 남아 있는 동안 반복
        if g1[0] < g2[0]:  # 두 그룹의 맨 앞 자료 값을 비교
            # g1 값이 더 작으면 그 값을 빼내어 결과로 추가한다.
            result.append(g1.pop(0))
        else:
            # 그 반대면 g2에
            result.append(g2.pop(0))
        # 아직 남아 있는 자료들을 result에 추가
        # g1과 g2 중 이미 빈 것은  while을 바로 지나감
    while g1:
            result.append(g1.pop(0))
    while g2:
            result.append(g2.pop(0))
    return result


d = [6, 8, 4, 9, 10, 1, 2, 3, 7, 5]

print(mergeSort(d))
```

이제 좀 이해가 가나?  위의 코드를 전에 했던 재귀 호출의 3가지 요건으로 정리를 해보자.

1. 병합 정렬은 자료 10개를 정리하기 위해서 재귀 호출한다.
2. 10개짜리 List를 반으로 나눈다.
3. 종료 조건은 입력 리스트에 자료가 1 개뿐이거나, 아예 자료가 없다면 끝낸다.

이해를 했다면 이제 일반적인 병합 정렬 알고리즘을 만들어보자. 위의 코드랑 다른 점은 `return`값이 없고, 입력 리스트 안의 자료 순서를 직접 바꿔준다는 것이다.

*normalMergeSort.py*

```python
# merge sort
# input : list a
# output : sorted list a

def mergeSort(a):
    n = len(a)
    
    # end condition : 정렬할 리스트의 자료 개수가 한 개 이하이면 정렬할 필요가 없다.
    if n <= 1:
        return
    
    # 그룹을 나누어 각각 병합 정렬하는 과정
    mid = n // 2
    g1 = a[:mid]
    g2 = a[mid:]

    mergeSort(g1)
    mergeSort(g2)
	
    # 순서 번호역할 할 변수들
    z = 0 
    x = 0 
    c = 0 

    while z < len(g1) and x < len(g2):
        if g1[z] < g2[x]:
            a[c] = g1[z]
            z += 1
            c += 1
        else:
            a[c] = g2[x]
            x += 1
            c += 1
    # 아직 남아 있는 자료들을 결과에 추가
    while z < len(g1):
        a[c] = g1[z]
        z += 1
        c += 1
    while x < len(g2):
        a[c] = g2[x]
        x += 1
        c += 1


d = [6, 8, 4, 9, 10, 1, 2, 3, 7, 5]

mergeSort(d)

print(d)
```



이렇게 병합정렬은 주어진 문제를 절반으로 나눠서 재귀 형식으로 풀어가는 정렬방식인데, 이를 분할 정복이라고 하기도 합니다.

분할 정복을 이용한 병합 정렬의 계산 복잡도는 `O(n*logn)`으로 낮은 편입니다.



#### 연습문제

위의 병합정렬 알고리즘은 오름차순 정렬이다. 이제 우리는 코드의 한 부분만 바꿔서 내림차순정렬로 바꿔보자.

*mergeSortDesc.py*

```python
# merge sort
# input : list a
# output : sorted list a

def mergeSort(a):
    n = len(a)
    # end condition : 정렬할 리스트의 자료 개수가 한 개 이하이면 정렬할 필요가 없다.

    if n <= 1:
        return
    # 그룹을 나누어 각각 병합 정렬하는 과정
    mid = n // 2
    g1 = a[:mid]
    g2 = a[mid:]

    mergeSort(g1)
    mergeSort(g2)

    z = 0 # i1
    x = 0 # i2
    c = 0 # ia

    while z < len(g1) and x < len(g2):
        if g1[z] > g2[x]: # 여기만 바꿨다!
            a[c] = g1[z]
            z += 1
            c += 1
        else:
            a[c] = g2[x]
            x += 1
            c += 1

    # 아직 남아 있는 자료들을 결과에 추가
    while z < len(g1):
        a[c] = g1[z]
        z += 1
        c += 1
    while x < len(g2):
        a[c] = g2[x]
        x += 1
        c += 1


d = [6, 8, 4, 9, 10, 1, 2, 3, 7, 5]

mergeSort(d)

print(d)
```

