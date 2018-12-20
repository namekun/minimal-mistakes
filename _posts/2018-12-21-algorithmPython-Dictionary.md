---
title: "모두의 알고리즘 with python - Dictionary"
tags:

  - python
  - algorithm
  - 모두의알고리즘
comments: true
---


# 동명이인 찾기 2~

이전에 문제에서 살펴본 동명이인 찾기문제를 이번에는 딕셔너리를 이용해서 풀어보고자 한다.
문제를 풀기전에, 우선 딕셔너리라는게 무엇인지 알아보자.

파이썬의 딕셔너리는 정보를 찾는 기진이 되는 키와 그 키에 연결된 값의 대응 관계를 저장하는 자료구조이다.(엥 이거 완전 Hash Table아니냐?)

예를 들어, 여러 사람이 있을 때, 각 사람의 이름(키)과 나이(값)를 대응시켜 딕셔너리로 쉽게 표현할 수 있다.

*python Console*

```python
d = {"Justin": 13, "John": 10, "Mike": 9}
d["Mike"] # 딕셔너리에서 값을 빼오는 방법
>>>9
d["Summer"] # 딕셔너리에 없는 값을 찾을 때 에러 발생
Traceback (most recent call last):
  File "<input>", line 1, in <module>
KeyError: 'Summer'
    
d["Summer"] = 1 # 딕셔너리에 새 값 추가
d["Summer"]
>>>1
d["Summer"] = 20 # 기존 값 수정.
d["Summer"]
>>>20
d
>>>{'Justin': 13, 'John': 10, 'Mike': 9, 'Summer': 20}
```

정보가 들어있지 않은 빈 딕셔너리를 만드려면 그냥 `d = {}` 또는 `d = dict()`이라고 적어주면된다.

딕셔너리의 이해를 더 높여보고자 예시로 학생명부를 만들어보자.

```python
s_info = {
    1: "김남혁",
    2: "현동민",
    3: "고은총"
}

print(s_info[2])

s_info[4] = "최재원" # 값을 하나 넣기 위해선 다음과 같이 key값과 value를 입력해준다.

print(s_info)

del s_info[4] # 하나의 값을 지우기 위해선 다음과 같은 del 명령어를 사용한다.

print(s_info)
```

### 딕셔너리 정리

|      함수      |                             설명                             |
| :------------: | :----------------------------------------------------------: |
|     len(a)     |              딕셔너리 길이(자료 개수)를 구한다.              |
|     d[key]     |          딕셔너리에서 키에 해당하는 값을 읽어온다.           |
| d[key] = value | 키에 값을 저장합니다. 없다면 새로 만들고 이미 있다면 기존 value에 덮어쓴다. |
|   del d[key]   |                  키에 해당하는 값을 지운다.                  |
|    clear()     |            딕셔너리에 담긴 모든 자료를 지웁니다.             |
|    key in d    |   키가 딕셔너리 d안에 있는지 확인한다. 반대는 key not in d   |

### 딕셔너리를 활용한 동명이인 찾기 알고리즘

딕셔너리를 어떻게 사용해야 동명이인 찾기 알고리즘를 효율적으로 만들 수 있을까?

힌트를 주겠다.

각 이름을 키로, 그 이름이 리스트에 등장한 횟수를 값으로 보면 된다. 당장 구현해보자

```python
def findSame(a):
    name_dict = dict()
    for x in a:
        if x in name_dict:
            name_dict[x] += 1
        else:
            name_dict[x] = 1

    result = set()

    for name in name_dict:
        if name_dict[name] >= 2:
            result.add(name)

    return result


name = ["tom", "john", "joe", "tom"]

print(findSame(name))
```

해당 알고리즘의 계산 복잡도는 `for`문이 2번들어갔으나, 중첩되지 않았기에 `O(n)`이다.

### 연습문제

다음과 같이 학생 번호와 이름이 주어졌을 때, 학생번호를 입력하면 그 학생 번호에 해당하는 이름을 돌려주고, 해당하는 학생 번호가 없으면 물음표를 돌려주는 코드를 짜보시오

```python
# 39 : Justin
# 14 : John
# 67 : Mike
# 105 : Summer


stuInfo = {
    39: "Justin",
    14: "John",
    67: "Mike",
    105: "Summer"
}


def findStu(num):
    if num in stuInfo:
        return stuInfo[num]
    else:
        return "?"


print(findStu(14)) # John
print(findStu(68)) # ?
```



### 시간 복잡도, 그리고 공간 복잡도

계산 복잡도에는 계싼을 얼마나 빨리 할 수 있는지 따져 보는 '시간 복잡도'와 '계산 복잡도'가 있습니다. 앞에서는 주로 시간 복잡도만 생각해서 말을 했었죠.

딕셔너리를 이용해 동명이인을 찾는 문제는 모든 사람을 서로 비교하는 방법보다 더 나은 시간 복잡도를 가진다. 하지만, 딕셔너리를 만들어 그 안에 모든 이름과 등장 횟수를 저장해야하므로 더 많은 저장 공간을 사용한다. 이것은 공간 복잡도를 희생하여 시간 복잡도를 개선한 것이라고 생각하면 된다.

알고리즘 분석을 정확하게 하려면 시간 복잡도뿐 아니라, 공간 복잡도도 함께 고려해줘야한다. 하지만 현대 컴퓨터는 대체로 저장 공간(메모리, 하드 디스크)이 매우 크기에, 상대적으로 공간 복잡도에 덜 민감하다고 할 수 있다. 
