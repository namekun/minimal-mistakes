---
title: "프로그래머스 코딩테스트 연습 문제 - Hash - 완주하지 못한 선수"
tags:
  - python
  - algorithm
  - hash
  - 프로그래머스
comments: true
---

### 문제 설명

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

### 입출력 예

| participant                             | completion                       | return |
| --------------------------------------- | -------------------------------- | ------ |
| [leo, kiki, eden]                       | [eden, kiki]                     | leo    |
| [marina, josipa, nikola, vinko, filipa] | [josipa, filipa, marina, nikola] | vinko  |
| [mislav, stanko, mislav, ana]           | [stanko, ana, mislav]            | mislav |

### 입출력 예 설명

예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

---

처음엔 그냥 간단하게 생각했다. 동명이인이 있다는 조건을 못봤어서;

```python
def solution(participant, completion):
    for x in participant:
        if x not in completion:
            answer = x

    return answer
```

이렇게 짰더니 동일 인물이 있는 예시에서  `UnboundLocalError`가 난다.

`hash`문제라는 것을 까먹고 있었다. 파이썬의 `hash`인 `dictionary`를 이용해보자. 말로 알고리즘을 우선 만들자.

1. 딕셔너리를 하나 만들고, `participant`에 있는 데이터를 넣는다. `이름-횟수`의 관계로.
2. `completion`에 있는 이름이 딕셔너리에 있다면 해당 value를 -1 해준다.
3. 딕셔너리에서 value가 1인 것을 찾아서 출력해준다.

이제 코드를 만들어보자. 위에 만들었던 순서대로.

```python
def solution(participant, completion):

    n = len(participant)
    nameDict = dict()

    for x in range(0, n):
        if participant[x] not in nameDict:
            nameDict[participant[x]] = 1
        else:
            nameDict[participant[x]] += 1

    for y in completion:
        if y in nameDict:
            nameDict[y] -= 1

    for z in nameDict:
        if nameDict[z] == 1:
            answer = z

    return answer
```

코드가 좀 지저분하긴한데^^; 그래도 정확성, 효율성 테스트 모두 통과했다!

풀 었 다!
