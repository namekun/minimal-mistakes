---
title: "모두의 알고리즘 with python - day1"
tags:
  - python
  - algorithm
  - 모두의알고리즘
comments: true
---

## 본격적으로 알고리즘 문제를 풀기전의 두뇌 풀기

## 1. List

### python의 리스트는 자바랑 거의 똑같다고 보면 편하다

```python
a= [5,7,8]

print(a)
print(a[0])
print(a[-1])
```

### list에서 자주 사용하는 함수는 다음과 같다.
* len(a) = 리스트 a의 길이를 구한다.
* append(a) = 자료 a를 리스트의 맨 뒤에 추가한다.
* insert(i, x) = 리스트의 i번 위치에 x를 추가한다.
* pop(i) = i번 위치에 있는 자료르 리스트에서 빼내면서 그 값을 함수의 결과값으로 돌려준다.
* clear() = 리스트의 모든 자료를 지운다.
* x in a = 어떤 자료 x가 리스트 a 안에 있는지 확인한다.
* x not in a = 자료 x가 리스트 a에 없는지 확인. return 값은 둘다 boolean

*list 를 이용한 최대값 찾기 문제*

```python
# 리스트에 숫자가 n개 있을 때, 가장 큰 값이 있는 위치 번호를 돌려주는 알고리즘을 만드시오.

def find_max(a):
    n = len(a)

    max_idx = 0

    for i in range(1, n):
        if a[i] > a[max_idx]:
            max_idx = i
    return max_idx


v = [17, 92, 18, 33, 58, 7, 33, 42]

print(find_max(v))
```

*연습문제*

```python
# 숫자 n개를 리스트로 입력받아 최솟값을 구하는 프로그램을 만들어 보세요


def find_min(a):
    n = len(a)
    min_v = a[0]

    for i in range(1, n):
        if a[i] < min_v:
            min_v = a[i]
    return min_v


v = [17, 92, 18, 33, 58, 7, 33, 42]

print(find_min(v))

```

---

## 2. set

### 집합(set)은 리스트와 같이 정보를 여러개 넣어서 보관할 수 있는 파이썬의 기능이다.
### 다만 집합 하나에는 같은 자료가 중복되어 들어가지 않고, 자료의 순서도 의미가 없다는 점이 리스트와 다르다.
```python
s = set()
s.add(1)
s.add(2)
s.add(2) # 이미 있는 값이기에 중복되서 들어가지 않는다.

print(s)

len(s)
```
* len(x) : 집합의 길이를 구하는 방법
* add(x) : 집합에 자료 x를 추가합니다.
* discard(x) : 집합에 자료 x가 들어 있다면 삭제합니다. 없다면 변화없음
* clear() : 집합의 모든 자료를 지운다.
* x in s : 어떤 자료 x가 집합 s에 들어가 있는지 확인, 반대는 not in

*동명이인 찾기 문제*

```python
# 두번 이상 나온 이름 찾기
# 입력 : 이름이 n개 들어있는 리스트
# 출력 : 이름 n개 중 반복되는 이름의 집합

def findSameName(a):
    n = len(a)

    result = set()
    for i in range(0, n - 1):
        for j in range(i + 1, n):
            if a[i] == a[j]:
                result.add(a[i])
    return result


name = ['Tom', 'Jerry', 'Mike', 'Tom']

print(findSameName(name))
```

---

## 3. Recursive

재귀 호출은 일반적으로 자기 자신을 다시 호출해서 문제를 해결하는 방식을 말한다.

재귀 호출의 기본적인 형태는 다음과 같다.

```python
def func(input):
	if 입력값이 충분히 작다면 # 종료조건
    	return 결과값
    ...
    func(더 작은 input)
    ...
    return 결과값
```

여기서 중요한 것은 재귀호출에는 항상 종료값이 필요하다는 것이다. 종료 조건이 없으면 재귀 에러(RecursionError)나 스택오버플로 등 프로그램 에러가 발생해 비정상적인 동작을 할 수 있다.

*Factorial_UseRecursive.py*

```python
# 연속한 숫자의 곱을 구하는 알고리즘
# 입력 : n
# 출력 : 1부터 n까지 연속한 숫자를 곱한 값


def fac(n):
    if n <= 1:
        return 1
    return n * fac(n - 1)


print(fac(5))
```

*FindMax_UseRecursive.py*

```python
# 숫자 n개 중에서 최댓값 찾기를 재귀 호출로 만들어 보자

def findMaxRec(a, n):
    if n == 1:
        return a[0]
    maxV = findMaxRec(a, n - 1)
    if maxV > a[n - 1]:
        return maxV
    else:
        return a[n - 1]


v = [34, 32, 12, 6, 7, 68, 70, 768, 54]

print(findMaxRec(v, len(v)))
```

---

## 4. GCD

최대 공약수는 두개 이상의 정수의 공통의 약수 중에서 가장 큰 값을 의미한다.

그래서 원하는 최대공약수를 찾기 위해서는

1. 두 수의 약수 중에서
2. 공통된 것을 찾아
3. 그 값중 최대값을 찾아야 한다.

위의 규칙을 활용해서 우리는 최대공약수를 구하는 알고리즘을 짜볼 것이다.

우선 알고리즘을 짜기전에 어떻게 짜야할 지 생각해보자

1. 두 수 중 더 작은 값을 i에 저장
2. i가 두 수의 공통된 약수인지 확인한다.
3. 공통된 약수이면 이 값을 결과값으로 돌려주고 종료한다.
4. 공통된 약수가 아니면 i를 1만큼 감소시키고 2번으로 돌아가서 반복한다. (1은 모든 정수의 약수이므로, i가 1이 되면 1을 결과값으로 돌려주고 종료한다.)

규칙까지 짰으면, 이제 알고리즘을 만들어보자

*GCD.py*

```python
def gcd(a, b):
    i = min(a, b)
    while True:
        if a % i == 0 and b % i == 0:
            return i
        else:
            i -= 1

print(gcd(81, 39))
```

### 유클리드 호제법

유클리드는 최대공약수에 다음과 같은 규칙이 있는 것을 발견했다. a와 b의 최대공약수는 b와 a를 b로 나눈 나머지의 최대공약수와 같다. 

즉 gcd(a, b) = gcd(b, a%b)인것!
어떤 수와 0의 최대 공약수는 자기 자신이다. 즉 gcd(n, 0) = n이다.
이 방식을 이용해서 재귀형식의 최대공약수 구하는 알고리즘을 만들어보자

*Euclid.py*

```pytho
def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a % b)


print(gcd(81, 39))
```

### 재귀호출의 이해를 돕는 방법

자신이 자신을 호출한다는 개념을 이해했다고 해도, 막상 짜보려고 하면 머릿속에 혼돈이 온다. 이럴땐 이렇게해보자

1. 종이에 직접 함수의 호출과정을 써본다. 이해에 도움이 된다.
2. 예제로 사용할 값은 작은 값부터 시작하도록 한다. 그 뒤 값을 천천히 늘려가보자.
3. 합수의 입력 값을 화면에 출력하는 것도 도움이 된다. 함수의 진행 단계마다 일일히 `print`해주면 해당 함수가 어떻게 돌아가는지 눈으로 직접 확인할 수 있다.

---

## 5. 하노이의 탑 - Line 기출 문제

하노이의 탑은 나름 유명한 문제이다.

이것을 우리가 배워온 것들을 통해서 풀어보도록 하자. 정 이해가 가지않는다면, 10, 100, 500원짜리 3개를 들고 종이위에서 고뇌해보도록 하자.

#### 하노이의 탑 규칙

* 크기가 다른 원반 n개를 출발점 기둥에서 도착점 기둥으로 전부 옮겨야한다.
* 원반은 한 번에 한 개씩만 옮길 수 있다.
* 원반을 옮길 때는 한 기둥의 맨 위 원반을 뽑아서, 다른 기둥의 맨 위로만 옮길 수 있다.
* 원반을 옮기는 과정에서 큰 원반을 작원 원반 위로 올릴 수 없다.

#### 하노이의 탑 푸는 방법

* 우선 1, 2개 층의 탑이 어떻게 1번기둥에서 3번기둥으로 움직이는지 해당 규칙부터 파악해본다.
* 그 후 3개짜리가 어떻게 그대로 옮겨질 수 있는지 파악한다.
* 파악한 규칙을 통해서 n층의 하노이의 탑을 옮기는 코드를 짜보자.

#### 하노이의 탑 알고리즘
1. 원반이 1개이면 그냥 옮기면 된다.
2. 원반이 n개라면
  1. 1번 기둥에 있는 n개중 n-1개의 원반을 2번 기둥으로 옮긴다.(3번 기둥을 보조 기둥으로 사용)
  2. 1번 기둥에 남아 있는 가장 큰 원반을 마지막 기둥으로 옮긴다.
  3. 2번 기둥에 있는 n-1개의 원반을 다시 3번 기둥으로 옮긴다.(1번 기둥을 보조 기둥으로 사용)

이제 직접 만들어보자. 아래의 코드는 탑이 옮겨지는 순서를 보여준다.

*TowerOfHanoi.py*

```python
# 입력 : 옮기려는 원반의 수 n
#        옮길 원반이 현재 있는 출발점 기둥 from_pos
#        원반을 옮길 도착점 기둥 to_pos
#        옮기는 과정에서 사용할 보조 기둥 aux_pos
# 출력 : 원반을 옮기는 순서

def hanoi(n, from_pos, to_pos, aux_pos):
    if n == 1: # 원반 한 개를 옮기는 문제면 그냥 옮겨준다.
        print(from_pos, "=>", to_pos)
        return
	
    # 원반 n-1개를 aux_pos로 이동(to_pos를 보조 기둥으로)
    hanoi(n - 1, from_pos, aux_pos, to_pos)
    # 가장 큰 원반을 목적지로
    print(from_pos, "=>", to_pos)
    # aux_pos에 있는 원반 n-1개를 목적지로 이동(from_pos를 보조 기둥이로) 
    hanoi(n - 1, aux_pos, to_pos, from_pos)


print("n = 1")
hanoi(1, 1, 3, 2) # 원반 한 개를 1번 기둥에서 3번 기둥으로 이동
print()
print("n = 2") 
hanoi(2, 1, 3, 2) # 원반 두 개를 1번 기둥에서 3번 기둥으로
print()
print("n = 3")
hanoi(3, 1, 3, 2) # 원반 3개를 1번에서 3번 기둥으로
```

*result*

```
n = 1
1 => 3

n = 2
1 => 2
1 => 3
2 => 3

n = 3
1 => 3
1 => 2
3 => 2
1 => 3
2 => 1
2 => 3
1 => 3
```

#### 알고리즘 분석

입력과 크기, 즉 탑의 층수가 높을 수록 원반을 더 많이 움직여야 한다는 것을 알 수 있다. 

n층의 하노이의 탑을 옮기려면 원반을 모두 `2^n-1`번 움직여야 한다는 규칙도 얻어낼 수 있다.

그러므로 해당 알고리즘의 시간 복잡도는 `O(2^n)`이라고 할 수 있다.

---

## 6. 순차 탐색

순차 탐색은 리스트 안에 있는 원소를 하나씩 순차적으로 비교하면서 찾는다. 같은 이름으로 선형 탐색(linear search)이라고 하기도 한다.

#### 순차 탐색으로 특정 값의 위치 찾기

문제를 푸는 방법은 매우 간단하다. 첫번째 데이터부터 하나씩 비교해가며 같은 값이 나오면 해당 위치를 결과로 돌려주고, 리스트 끝까지 나오지않는다면 -1을 돌려주면 된다.

```python
# list에서 특정 숫자의 위치 찾기
# input : 리스트 a , 찾는 값 x

def search(a, x):
    n = len(a)

    for i in range(0, n):
        if a[i] == x:
            return i

    return -1


a = [1, 3, 6, 8, 5]

print(search(a, 5))
```

위의 알고리즘의 계산 복잡도는 `O(n)`이다.

#### 연습문제

1. 찾는 값이 모두 나오는 순차 정렬 알고리즘 만들기

*seqSearch+.py*

```python
# 찾는 값에 해당하는 모든 위치를 반환


def seq(a, x):
    n = len(a)
    b = []
    for i in range(0, n):
        if a[i] == x:
            b.append(i)
    return b


a = [1, 23, 4, 5, 6, 4, 8, 4]

print(seq(a, 10))
```

2. 학생 번호를 입력하면 학생의 이름을 찾아주는 알고리즘

*findStu.py*

```python
def findS(a, b, x):
    n = len(a)

    for i in range(0, n):
        if x == a[i]:
            return b[i]


stu_no = [39, 14, 67, 105]
stu_name = ['justin', 'john', 'mike', 'Summer']

print(findS(stu_no, stu_name, 14))
```

---

## 7. 선택 정렬

선택 정렬은 쉽게 말해서 주어진 리스트에서 가장 작은 값을 빼온 뒤, 이를 순서대로 배열한 정렬된 리스트를 하나 만들어주는 것을 말한다.

말 그대로 '선택'해서 '정렬'한 것이다.

가장 쉬운 선택정렬 알고리즘을 구현해본다.

*easySelectionSort.py*

```python
# 선택 정렬은 주어진 리스트에서 가장 작은 것들을 '선택'해서 정렬해준다는 의미.
# 입력 : 리스트 a
# 출력 : 정렬된 새 리스트
# 주어진 리스트에서 최솟값의 위치를 돌려주는 함수

def find_min_idx(a):
    n = len(a)
    min_idx = 0

    for i in range(1, n):
        if a[i] < a[min_idx]:
            min_idx = i
    return min_idx

def sel_sort(a):
    result = [] # 새 리스트를 만들어 정렬된 값을 저장
    while a: # 주어진 리스트에 값에 남아 있는 동안 계속
        min_idx = find_min_idx(a) # 리스트에 남아 잇는 값 중 최솟값의 위치를 찾고
        value = a.pop(min_idx) # 찾은 최솟값을 빼내어 value에 저장
        result.append(value) # value를 결과 리스트 끝에 추가
    return  result

d = [2, 4, 5, 1, 3]
print(sel_sort(d))
```



위의 코드를 이해 햇다면, 이제 일반적인 선택 정렬 알고리즘을 만들어보자.

*selSortPlus.py*

```python
# 선택정렬
# 입력 : 리스트 a
# 출력 : 없음(입력으로 주어진 a가 정렬된다)

def selSort(a):
    n = len(a)
    for i in range(0, n - 1):  # 0부터 n-2까지 반복
        # i번 위치부터 끝까지 자료 값 중에 최솟값의 위치를 먼저 찾는다.
        min_idx = i
        for j in range(i + 1, n):
            if a[j] < a[min_idx]:
                min_idx = j
        # 찾은 최솟값을 i번 위치로
        a[i], a[min_idx] = a[min_idx], a[i]


d = [2, 4, 5, 1, 3]
selSort(d)

print(d)
```

\* 추가로, 위의 코드에서 ` a[i], a[min_idx] = a[min_idx], a[i]`부분을 리스트 안의 두 자료 값의 위치를 변경하는데 사용하였는데, 파이썬에서는 이렇게 두 변수의 값을 서로 바꾸려면 저렇게 쉼표를 이용해 변수를 뒤집어 표현하면된다.

```python
x, y = y, x
```

#### 알고리즘 분석

자료를 크기 순서로 정렬하려면 반드시 두 수의 크기를 비교해야한다. 따라서 정렬 알고리즘의 계산 복잡도는 보통 비교 횟수를 기준으로 따지는데, 선택 정렬의 비교 방법은 리스트 안의 자료를 한 번씩 비교하는 방법과 거의 같기에, 계산 복잡도는 `O(n^2)`라고 할 수 있다.

*내림차순 선택정렬*

```python
# 오름차순이 아닌 내림차순으로 정렬하기

def selSort(a):
    n = len(a)
    for i in range(0, n - 1):  # 0부터 n-2까지 반복
        # i번 위치부터 끝까지 자료 값 중에 최대값의 위치를 먼저 찾는다.
        max_idx = i
        for j in range(i + 1, n):
            if a[j] > a[max_idx]:
                max_idx = j
        # 찾은 최대값을 i번 위치로
        a[i], a[max_idx] = a[max_idx], a[i]


d = [2, 4, 5, 1, 3]
selSort(d)

print(d)
```

---

## 8. 삽입 정렬

삽입 정렬이 선택정렬과 다른 점은, 선택은 최소값을 고르고, 이를 뽑아서 정렬하지만, 삽입은 순서상관없이 뽑고, 이것을 뽑아놓은 아이들사이에서 비교해서 적절한 위치에 '삽입'하는 것이다.

알기 쉬운 삽입정렬 코드를 보자

*easyInsertionSort.py*

```python
# 쉽게 설명한 삽입 정렬
# 입력 : 리스트 a
# 출력 : 정렬된 새 리스트

# 리스트 r에서 v가 들어가야 할 위치를 돌려주는 함수
def find_ins_idx(r, v):
    # 이미 정렬된 리스트 r의 재료를 앞에서부터 차례로 확인하여
    for i in range(0, len(r)):
        # v 값보다 i번 위치에 있는 자료 값이 크면
        # v가 그 값 바로 앞에 놓여야 정렬 순서가 유지됨
        if v < r[i]:
            return i
    # 적절한 위치를 못 찾았을 때는
    # v가 r의 모든 자료보다 크다는 뜻이므로 맨 뒤에 삽입
    return len(r)


def ins_sort(a):
    result = []  # 새 리스트를 만들어 정렬된 값을 저장
    while a:  # 기존 리스트에 값이 남아 있는 동안 반복
        value = a.pop(0)  # 기존 리스트에서 한개를 꺼냄
        ins_idx = find_ins_idx(result, value)  # 꺼낸 값이 들어갈 적당한 위치 찾기기
        result.insert(ins_idx, value)  # 찾은 위치에 값 삽입.(이후에 값은 한 칸씩 밀려남)
    return result


d = [2, 4, 5, 1, 3]

print(ins_sort(d))
```



위의 코드를 이해했다면, 이제 일반적인 삽입정렬 코드를 만들어보자

*insertionSort.py*

```python
# 삽입정렬
# 입력 : 리스트 a
# 출력 : 없음 (입력으로 주어지는  a가 정렬된다.)


def insSort(a):
    n = len(a)
    for i in range(1, n):
        key = a[i]  # i번 위치에 있는 값을 key에 저장
        # j를 i 바로 왼쪽위치로
        j = i - 1
        # 리스트의 j번 위치에 있는 값과 key를 비교해 key가 삽입될 적절한 위치를 찾는다.
        while j >= 0 and a[j] > key:
            a[j + 1] = a[j]  # 삽입할 공간이 발생하도록 오른쪽으로 한 칸 이동한다.
            j -= 1
        a[j + 1] = key  # 찾은 삽입 위치에 key를 저장


d = [2, 4, 5, 1, 3]
insSort(d)

print(d)
```



위 알고리즘의 시간복잡도는 최소 `O(n)`이고, 최대 `O(n^2)`이다. 따라서 선택 정렬과 마찬가지로 입력크기가 커지면, 정렬하는데 꽤나 시간을 잡아 먹는 경우가 발생한다.

*연습문제*

```python
# 내림차순의 삽입정렬 구현하기

def insSortDesc(x):
    for i in range(1, len(x)):
        val = x[i]
        j = i - 1
        # desc의 경우에는 작은수가 큰수의 뒤로 가야하기 때문에 아래와 같이 바꿔줘야한다.
        while j >= 0 and x[j] < val:
            x[j + 1] = x[j]
            j -= 1
        x[j + 1] = val

    return x

d = [2, 1, 5, 3, 4]

print(insSortDesc(d))
```

