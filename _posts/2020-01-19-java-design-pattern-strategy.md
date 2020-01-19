취업후, 한동안 너무 바뻐서 못하다가 노트북을 새로샀음에도 불구하고 뭔가는 해야하지 않을까! 싶어서... 공부를 다시 시작하기로 했다.

공부를 뭘 해야하나 싶다가 가장 부족하지만 가장 많이 사용하고 있는 Java 에 대해서 공부해보기로 한다.

# Java Design Pattern First Class

---
title: "java-design-pattern-strategy"
tags:
  - Java
  - designPattern
  - gof
comments: true
---


## Interface

- 키보드나 디스플레이 따위처럼 사람과 컴퓨터를 연결하는 장치
- Java 에서의 interface?
    - 기능에 대한 선언과 구현 분리
    - 기능을 사용하는 통로

## Delegate

- 위임하다.
- 어느 function에 필요한 기능이 특정 인터페이스에 있다면?
    - 다른 객체에게 기능을 빌려서 가져온다. -> delegate
- 특정 객체의 기능을 사용하기위해, 다른 객체의 기능을 호출하는 것

## Strategy Pattern

- 여러 알고리즘의 추상적인 접근점을 만들어 접근 점에서 서로 교환 가능하도록 하는 패턴
- 설계

![https://gmlwjd9405.github.io/images/design-pattern-strategy/strategy-pattern.png](https://gmlwjd9405.github.io/images/design-pattern-strategy/strategy-pattern.png)

- 역할이 수행하는 작업
    - strategy
        - 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시
    - ConcreteStrategy
        - strategy패턴에서 명시한 알고리즘을 실제로 구현한 method
    - Context
        - strategy 패턴을 이용하는 역할을 수행
        - 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메서드(*집약관계*)를 제공한다.

## 구현해보자!

### 요구사항

- 신작 게임에서 캐릭터와 무기를 구현해보자
- 무기에는 총과 검 두가지 종류가 있다.

*weapon.interface - startegy*

    public interface Weapon {
        // 공격 기능
        public void attack();
    }

*Gun.java*

    public class Gun implements Weapon {
        @Override
        public void attack(){
            System.out.println("빵야빵야");
        }
    }

*Sword.java*

    public class Sword implements Weapon {
        @Override
        public void attack(){
            System.out.println("휙휙!");
        }
    }

*GameCharacter.java*

    public class GameCharacter {
        // 접근점
        private Weapon weapon;
    
        // 교환 가능
        public void setWeapon(Weapon weapon){
            this.weapon = weapon;
        }
    
        public void attack(){
            if(weapon == null){
                System.out.println("맨손 공격!");
            }
            else {
                // delegate
                weapon.attack();
            }
        }
    }

*Main.java*

    public class Main {
        public static void main(String[] args) {
            GameCharacter character = new GameCharacter();
    
            character.attack();
    
            character.setWeapon(new Gun());
            character.attack();
            character.setWeapon(new Sword());
            character.attack();
        }
    }

### 추가 요건

- 무기에 도끼를 추가해주세요

아주 간단하다! Ax.java만 추가해주면 되는 것!

*Ax.java*

    public class Ax implements Weapon {
        @Override
        public void attack(){
            System.out.println("도끼 공격!");
        }
    }
