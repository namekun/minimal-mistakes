---
title: "자바 디자인패턴 Bridge pattern"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## Bridge Pattern

구현부에서 추상층을 분리하여 각자 독립적으로 변형이 가능하고, 확장이 가능하도록하는 패턴

즉, 기능과 구현에 대해 두 개를 별도의 클래스로 구현한다.

* 브릿지 패턴의 구조

  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F999CAD3359C4C45D24D27F)
  * Abstraction : 기능 계층의 최상위 클래스. 구현 부분에 해당하는 클래스를 인스턴스를 갖고 해당 인스턴스를 통해 구현 부분의 메서드를 호출
  * RefindAbstraction :  기능 계층에서 새로운 부분을 확장한 클래스
  * Implementor : Abstraction의 기능을 구현하기 위한 인터페이스 정의
  * ConcreteImplementor : 실제 기능을 구현

* 브릿지 패턴의 예제

  각 동물 이라는 클래스와 이 동물 클래스가 가질 수 있는 '사냥방법'을 브릿지 패턴을 적용해서 각가 분리, 설계

  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F9942083359C4C59317D32B)

  기능부에 해당하는 최상위 클래스 animal 이 존재하고, 그 하위로 bird, tiger 가 존재,

  '동물' 이라는 추상 객체의 기능 구현 부분을 Hunting, Handler와 분리하여 구조 설계

* 코드

  *Animal.java*

  ```java
  public class Animal {
      private Hunting_Handler hunt;
      public Animal(Hunting_Handler hunt){
          this.hunt = hunt;
      }
  
      public void Find_Quarry(){
          hunt.Find_Quarry();
      }
  
      public void Detected_Quarry(){
          hunt.Detected_Quarry();
      }
  
      public void Attack(){
          hunt.attack();
      }
  
      public void hunt(){
          Find_Quarry();
          Detected_Quarry();
          Attack();
      }
  }
  ```

  * 기능 부분에 해당하는 최상위 클래스.
  * Hunting_Handler 의 인스턴스를 갖고, 각각의 Hunting_Handler 를 상속받아 구현하고 있는 메서드 호출

  *Hunting_Handler.interface*

  ```java
  public interface Hunting_Handler {
      public void Find_Quarry();
      public void Detected_Quarry();
      public void attack();
  }
  ```

  * 동물이 가질 수 있는 '사냥' 방식들이 가져야 할 공통 인터페이스 정의

  *Hunting_Method1.java*

  ```java
  public class Hunting_Method1 implements Hunting_Handler {
  
      public void Find_Quarry() {
          System.out.println("물 위에서 찾는다!");
      }
  
      public void Detected_Quarry() {
          System.out.println("물고기 발견!");
      }
  
      public void attack() {
          System.out.println("사냥 시작!");
      }
  }
  ```

  *Hunting_Method2.java*

  ```java
  public class Hunting_Method2 implements Hunting_Handler {
      public void Find_Quarry() {
          System.out.println("땅 위에서 찾는다!");
      }
  
      public void Detected_Quarry() {
          System.out.println("먹잇감 발견!");
      }
  
      public void attack() {
          System.out.println("사냥 시작!");
      }
  }
  ```

  * `Hunting_Handler` 인터페이스를 상속받아 실제 기능에 해당하는 부분을 구현

  *Tiger.java*

  ```java
  public class Tiger extends Animal {
      public Tiger(Hunting_Handler hunt) {
          super(hunt);
      }
  
      public void hunt(){
          System.out.println("호랑이가 사냥한다 어흥");
          Find_Quarry();
          Detected_Quarry();
          Attack();
      }
  }
  ```

  * Animal 을 확장한 클래스 

  *Bird.java*

  ```java
  public class Bird extends Animal {
      public Bird(Hunting_Handler hunt) {
          super(hunt);
      }
      public void hunt(){
          System.out.println("새가 사냥한다 짹짹");
          Find_Quarry();
          Detected_Quarry();
          Attack();
      }
  }
  ```

  *Main.java*

  ```java
  public class Main {
      public static void main(String[] args) {
          Animal tiger = new Tiger(new Hunting_Method2());
          Animal bird = new Bird(new Hunting_Method1());
  
          tiger.hunt();
          System.out.println("-------------------------");
          bird.hunt();
      }
  }
  ```
