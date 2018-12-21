---
title: "모두의 알고리즘 with python - Graph"
tags:

  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# Graph

이번에는 친구 관계를 이용해서 어떤 한 사람의 '모든 친구'를 출력하는 알고리즘을 만들어보자.

만들기 전에 우리는 이때 꼭 필요한 자료구조인 '그래프'에 대해서 알아봐야한다.

우리가 학창시절에 그렸던 뭐.. x축과 y축을 기반으로 하는 그런 그래프가 아니고, 구냥 꼭짓점(vertex)들끼리 있는 선(edge)을 말한다.

이제 파이썬으로 해당 그래프들을 만들어보자.

파이썬에서 그래프를 자료구조로 만들어 저장하는 방법은 여러가지가 있지만, 난 내가 이미 아는 리스트와 딕셔너리를 이용해서 만들어보려고 한다.

일단 그래프를 표현하려면 각 꼭짓점의 정보부터 저장해야한다. 그래프를 표현할 `freInfo`딕셔너리를 만들고 키를 각 꼭짓점으로 지정한다. 그 뒤에는 키인 이름에 해당하는 꼭짓점인 친구들을 입력해줍니다.

```python
freInfo = {
    'summer': ['john', 'justin', 'mike'],
    'john': ['summer', 'justin'],
    'justin': ['john', 'summer', 'mike', 'may'],
    'mike': ['summer', 'justin'],
    'may': ['justin', 'kim'],
    'kim': ['may'],
    'tom': ['jerry'],
    'jerry': ['tom']
}
```

이제 이걸 기반으로 알고리즘을 만들어보자.

### 모든 친구 찾기 알고리즘

주어진 친구 관계에 있어 누군가 한명의 친구를 출력하고, 그 친구들의 친구들까지 모두 출력해보고 싶다. 이를 구현하려면 어떻게 해야할까?

우선, 꼬리에 꼬리를 무는 친구들을 모두 처리하려면, 모든 친구를 다 적어뒀다가 하나하나 처리를 해줘야한다.

그리고 이렇게 처리하는 친구들 중에, 이미 앞서 처리된 친구는 반복되지않도록 따로 메모해두어 무한반복이 발생하지 않도록 해야한다.

대충 감이 오는가?

앞으로 처리해야할 친구들은 큐에 넣는다. 그리고 이미 처리 대상으로 추가한 사람들은 집합을 이용해서 처리한다.(중복 자동 처리)

알고리즘은 다음과 같은 순서로 만든다.

1. 앞으로 처리할 사람을 저장할 큐를 만든다.

2. 이미 큐에 추가한 사람을 저장할 집합을 따로 만든다.

3. 검색의 출발점이 될 사람을 큐와 집합에 추가한다.

4. 큐에 사람이 남아있다면, 큐에서 처리할 사람을 계속 꺼낸다.

5. 꺼낸 사람을 출력한다.

6. 꺼낸 사람의 친구들 중, 아직 큐에 추가된 적이 없는 사람을 골라 큐와 집합에 추가한다.

7. 큐에 처리할 사람이 남아있다면, 4번부터 다시 반복

코드를 짜보자.

```python
def findFre(g, name):
    usedName = []
    already = set()

    usedName.append(name)
    already.add(name)

    while usedName:
        fre = usedName.pop(0)
        print(fre)
        for x in g[fre]:
            if x not in already:
                usedName.append(x)
                already.add(x)


freInfo = {
    'summer': ['john', 'justin', 'mike'],
    'john': ['summer', 'justin'],
    'justin': ['john', 'summer', 'mike', 'may'],
    'mike': ['summer', 'justin'],
    'may': ['justin', 'kim'],
    'kim': ['may'],
    'tom': ['jerry'],
    'jerry': ['tom']
}

findFre(freInfo, 'summer')
print()
findFre(freInfo,'tom')
```

이제 위의 알고리즘에 친밀도(촌수) 계산 기능까지 넣어보자.

### 친밀도 계산 알고리즘

예를 들어서 , A, B가 친구이고, B, C가 친구라고 가정해보자. A를 기준으로 B와의 친밀도는 1, B를 기준으로 C와의 친밀도 역시 1이다. 한편, A와 C는 B를 통해 친구의 친구가 되었기에 친밀도는 거리만큼 늘어나 2이다. A라는 사람과 X라는 사람의 친밀도가 n이면 X의 친구 Y는 A와의 친밀도가 n + 1이 된다.

이러한 성질을 이용해서 어떤 사람의 친구들을 큐에 넣을 때, 친밀도를 1씩 증가시키면 된다.

```python
# 친구리스트에서 자신의 모든 친구를 찾고 친구들의 친밀도를 계산하는 알고리즘
# 입력 : 친구 관계 그래프 g, 모든 친구를 찾을 name
# 출력 : 모든 친구의 이름과 자신과의 친밀도

def findFre(g, name):
    usedName = []
    already = set()

    usedName.append((name, 0)) # 친밀도와 이름을 함께 묶어서 튜플로 처리
    already.add(name)

    while usedName:
        (fre, d) = usedName.pop(0)
        print(fre, d)
        for x in g[fre]:
            if x not in already:
                usedName.append((x, d + 1))
                already.add(x)


freInfo = {
    'summer': ['john', 'justin', 'mike'],
    'john': ['summer', 'justin'],
    'justin': ['john', 'summer', 'mike', 'may'],
    'mike': ['summer', 'justin'],
    'may': ['justin', 'kim'],
    'kim': ['may'],
    'tom': ['jerry'],
    'jerry': ['tom']
}

findFre(freInfo, 'summer')
```

### 튜플?

여기서 갑작스럽게 사용한 튜플에 대해 알아보자.

튜플은 여러개의 정보를 묶어서 하나의 정보처럼 사용하기 위한 기능으로 수학에서 `(x, y)`처럼 표현하는 것이다.

튜플로 묶어서 보관하고자 하는 정보가 있다면, 소괄호 안에 쉼표`,`로 나열하면 된다. 어때요, 참 쉽죠?

```python
>>>t = (3, 6)
>>>t
(3, 6)
>>>t[0]
3
>>>t[1]
6
>>>(x, y) = t # 튜플 t안의 값들을 변수 x, y에 저장한다.
>>>x
3
>>>y
6

```

### 연습문제

간단한 그래프를 탐색하는 두 가지 그래프를 구현해보자.

#### BFS

```python
graphInfo = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1],
    4: [2],
    5: [2]
}

# BFS - 너비 우선 탐색
# 탐색의 순서 : 1, 2, 3, 4, 5


def bfs(g, start):
    visited = set()
    queue = []

    queue.append(start)
    visited.add(start)

    while queue:
        n = queue.pop(0)
        for x in g[n]:
            if x not in visited:
                queue.append(x)
                visited.add(x)
    return visited


print(bfs(graphInfo, 1))
```

### DFS

```python
# DFS - 깊이 우선 탐색
# 탐색 순서 : [1, 2, 4, 5, 3]

graphInfo = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1],
    4: [2],
    5: [2]
}


def dfs(g, start):
    stack = []
    visited = list()

    stack.append(start)

    while stack:
        n = stack.pop()  # 스택에서 하나 꺼낸다.
        visited.append(n)
        for x in g[n]:
            if x not in visited:
                stack.append(x)
    return visited


print(dfs(graphInfo, 1)) # [1, 3, 2, 5, 4]
```

스택을 이용했기때문에 거꾸로 나왔다.
