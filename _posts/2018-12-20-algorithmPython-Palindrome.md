---
title: "모두의 알고리즘 with python - Palindrome"
tags:

  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

# 회문 찾기

이번에 풀어볼 문제는 회문`Palindrom` 찾기 문제입니다. 

회문은 앞에서부터 읽어도 이효리 뒤에서 부터 읽어도 이효리 뭐 이런겁니다. 앞뒤로 읽어도 똑같은 글자를 말하는 것...

이것을 어떻게 파이썬에서 판단할 수 있을까? 우리는 이를 위해서 `Queue`와 `Stack`이라는 자료구조에 대해서 배워야 한다.

### 큐

우선 큐는 쉽게 말해서 '줄서기'라고 할 수 있다. 줄을 서는 모양을 생각해보자. 줄을 서는 이유는 빨리온 만큼 빠르게 무언가를 하기 위함이다. 큐는 이 정신을 그대로 갖고있다. FIFO. 즉, First In First Out이다. 자료를 큐에 집어넣으면, 빨리 온 순서대로 빼낼 수 있다.

자료를 집어넣는 동작을 우리는 보통 `enqueue`라고 하고, 빼내는 동작을 `dequeue`라고 한다.

### 스택

스택은 큐와는 다르다. 쉽게 생각하면 접시쌓기라고 생각하면 편하다. 접시를 쌓는다면, 아래서부터 빼내지않고 위에서부터 한 장씩 빼내는 것이 보통이다. 스택은 이와 똑같은 구조를 갖고있다. 큐와는 다르게, 처음 들어온 것이 가장 마지막에 나간다. LIFO, 즉, Last In First Out이다. 

스택에 자료를 집어 넣는 과정을 우리는 `push`, 그리고 빼내는 과정을 `pop`라고 한다.

### 리스트로 큐 와 스택 사용하기

| 자료 구조 |    동작    |     코드     |                 설명                 |
| :-------: | :--------: | :----------: | :----------------------------------: |
|    큐     | initialize |    q = []    |         빈 리스트를 만든다.          |
|           |  enqueue   |   q.append   |   리스트의 맨 뒤에 자료를 넣는다.    |
|           |  dequeue   | x = q.pop(0) | 리스트의 맨 앞(0)에서 자료를 꺼낸다. |
|  리스트   | initialize |   st = []    |         빈 리스트를 만든다.          |
|           |    push    |  st.append   |    리스트의 맨 뒤에서 자료를 추가    |
|           |    pop     | x = st.pop() |    리스트의 맨 뒤에서 자료를 꺼냄    |



## 회문 찾기 알고리즘

이제 회문 찾기 알고리즘에 큐와 스택의 특성을 이용해보자.

주어진 문장의 문자들을 하나하나 큐와 스택에 넣은 다음 큐와 스택에서 하나씩 자료를 꺼낸다고 생각해보자. 큐는 들어간 순서 그대로, 스택은 들어간 순서 정반대로 문자들이 뽑혀 나온다. 

회문은 거꾸로 읽어도 같은 글자가 나와야한다. 따라서 큐에서 꺼낸 문자들이 스택에서 꺼낸 문자들과 모두 같다면, 그 문장은 회문이다.

이 규칙을 통해서  회문 찾기 알고리즘을 구현해보자.

```python
def palindrom(s):
    queue = []
    stack = []

    for x in s:  # 굳이 parse해주지 않아도 된다.
        if x.isalpha():  # x가 알파벳인가?
            queue.append(x)
            stack.append(x)

    while queue:
        if queue.pop(0) != stack.pop():
            return False;

        return True


print(palindrom("wow")) # True
print(palindrom("dsfadsf")) # False
```

### 연습 문제

큐와 스택을 이용하지 않은 팰린드롬 알고리즘을 구현해보자.

```python
# 큐와 스택을 사용하지 않고 팰린드롬 알고리즘 구현하기


def palindrom(s):
    lenS = len(s)

    for x in range(lenS // 2):
        if s[x] != s[lenS - x - 1]:
            return False
    return True


print(palindrom("wow")) # True
print(palindrom("dsfadsf")) # False
```

