---
title: "자바 디자인패턴 Singleton"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

singleton을 자꾸 singletone이라고 쓴다;;; 

### 객체란

* 객체 :  속성과 기능을 갖춘 것
* 클래스 : 속성과 기능을 정의한 것
* 인스턴스 : 속성과 기능을 가진 것 중 실제하는 것

## Singleton

* Singleton : 요소를 1개밖에 가지고 있지 않은 집합
* 지정한 클래스의 인스턴스가 1개만 존재하는 것을 '보증'하고 싶을 때.
* 인스턴스가 1개밖에 존재하지 않는 것을 프로그램 상에서 표현하고 싶을 때.

### 사용방법

* 생성자를 private 로 선언하고, 해당하는 생성자를 클래스 내부에서만 호출

### Uml

![](https://upload.wikimedia.org/wikipedia/commons/d/dc/Singleton_pattern_uml.png)

### 요구사항

개발 중에 시스템에서 스피커에 접근할 수 있는 클래스를 만들어주세요.

### Code

*SystemSpeaker.java*

```java
public class SystemSpeaker {

    static private SystemSpeaker instance;

    private  int Volumne;

    private SystemSpeaker() {
        Volumne = 5; 
    }

    public static SystemSpeaker getInstance() {
        if (instance == null) {
            instance = new SystemSpeaker(); // initialize
            System.out.println("log 생성");
        }else{
            System.out.println("log 이미 생성");
        }
        return instance;
    }

    public int getVolumne() {
        return Volumne;
    }

    public void setVolumne(int volumne) {
        Volumne = volumne;
    }
}

```

*Main.java*

```java
public class Main {
    public static void main(String[] args) {
        SystemSpeaker speaker1 = SystemSpeaker.getInstance();
        SystemSpeaker speaker2 = SystemSpeaker.getInstance();

        System.out.println(speaker1.getVolumne());
        System.out.println(speaker2.getVolumne());

        speaker1.setVolumne(1);
        // 동일한 인스턴스 값이기에 하나만 설정해도 같은 값을 가져온다.
        System.out.println(speaker1.getVolumne());
        System.out.println(speaker2.getVolumne());

        speaker2.setVolumne(30);
        System.out.println(speaker1.getVolumne());
        System.out.println(speaker2.getVolumne());
    }
}
```

*Result*

```console
log 생성
log 이미 생성
5
5
1
1
30
30
```
