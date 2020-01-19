---
title: "자바 디자인패턴 Adapter"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

# Java Design Pattern Second Class

### Adapter

- 사전적 의미?
    - 기계, 기구 등을 다목적으로 사용하기 위한 부가 기구
- 자바에서의 의미?
    - 한 클래스의 인터페이스를 사용하고자 하는 다른 인터페이스로 변환할 때 주로 사용하며, 이를 이용하면 인터페이스의 호환성이 맞지 않아 같이 사용할 수 없는 class를 연관관계로 연결해셔사용할 수 있게 해주는 패턴
- 장점?
    - 관계가 없는 인터페이스를 같이 사용할 수 있다.
    - 프로그램 검사에 용이하다.
    - 클래스 재활용성 증가

[https://t1.daumcdn.net/cfile/tistory/99B1863B5AFA710332](https://t1.daumcdn.net/cfile/tistory/99B1863B5AFA710332)

### 요구사항

- 두 수에 대한 다음 연산을 수행하는 객체를 만들어주세요
    - 수의 두 배의 수를 반환 : twiceOf(Float) : Float
    - 수의 반(1/2) 의 수를 반환 : halfOf(Float) : Float
- 구현 객체의 이름은 Adapter로 해주세요
- Math 클래스에서 두 배의 절반을 구하는 함수는 이미 구현되어 있습니다.

### Adapter Code

*Adapter.interface*
```java

    public interface Adapter {
    
        // 원하는 기능
        public Float twiceOf(Float f);
        public Float halfOf(Float f);
    
    }
```    

*AdapterImpl.java*

```java
    public class AdapterImpl implements Adapter{
    
        @Override
        public Float twiceOf(Float f) {
    
            // 이런식으로 float 형태로 
    				// 변경해야하는 logic 이 필요
            return (float) Math.twoTime(f.doubleValue());
        }
    
        @Override
        public Float halfOf(Float f) {
            return (float) Math.half(f.doubleValue());
        }
    }
```
*Math.java*
```java
    public class Math {
        // 두배
        public static double twoTime(double num) {
            return num * 2;
        }
    
        // 절반
        public static double half(double num) {
            return num / 2;
        }
    }
```
*Main.java*
```java
    public class Main {
        public static void main(String[] args) {
            Adapter adapter = new AdapterImpl();
    
            System.out.println( adapter.twiceOf(100f));
            System.out.println( adapter.halfOf(100f));
        }
    }
```
### 추가해봅시다.

- 알고리즘 변경을 원합니다.
- Math 클래스에 새롭게 두 배를 구할 수 있는 함수가 추기되었습니다. 새로 구현된 알고리즘을 이용하도록 프로그램을 수정해주세요

*Math.java*
```java
    public class Math {
        // 두배
        public static double twoTime(double num) {
            return num * 2;
        }
    
        // 절반
        public static double half(double num) {
            return num / 2;
        }
    
        // enhanced algorithm
        public static Double doubled(Double d) {
            return d * 2;
        }
    }
```
*AdapterImpl.java*
```java
    public class AdapterImpl implements Adapter{
    
        @Override
        public Float twiceOf(Float f) {
    
            // 이런식으로 float 형태로 
    				// 변경해야하는 logic 이 필요
            return  Math.doubled(f.doubleValue()).floatValue();
        }
    
        @Override
        public Float halfOf(Float f) {
            return (float) Math.half(f.doubleValue());
        }
    }
```
