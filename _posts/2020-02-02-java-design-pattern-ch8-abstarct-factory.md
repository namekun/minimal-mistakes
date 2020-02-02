---
title: "자바 디자인패턴 abstarct factory pattern"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## abstarct factory pattern

### 추상 팩토리 패턴이란?

* 기존의 factory을 좀 더 생산적으로 만들어 낼 수 있다는 점 외에는 기존의 factory pattern과 거의 동일하다.
* 구체적인 클래스에 의존하지 않고 서로 연관되거나, 의존적인 객체들의 조합을 만드는 인터페이스를 제공한다.
  * 즉, 관련성있는 여러 종류의 객체를 일관된 방식으로 생성하는 경우에 유용
  * 싱글턴 패턴, 팩토리 메서드 패턴을 사용
  * '생성(Creational) 패턴' 중 하나
* Diagram

![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-pattern.png)

* 위에서 각자 수행하는 작업
  * AbstractFactory
    * 실제 팩토리 클래스의 공통 인터페이스
  * ConcreteFactory
    * 구체적인 팩토리 클래스로 AbstractFactory 클래스의 추상 메서드를 Override함으로써 구체적인 제품을 생성한다.
  * AbstaractProduct
    * 제품의 공통 인터페이스
  * ConcreteProduct
    * 구체적인 팩토리 클래스에서 생성되는 구체적인 제품

* 참고

  * 생성(Creational) 패턴
    * 객체 생성에 관련된 패턴
    * 객체의 생성과 조합을 캡슐화해 특저 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.

  

### EX - 엘리베이터 부품 업체 변경하기

* Diagram

  ![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-example.png)

  

* 여러 제조업체의 부품을 사용하더라고 같은 동작을 지원하게 하는 것이 바람직하다. 즉, 엘리베이터 프로그램의 변경을 최소화 해야한다.

  * 추상 클래스로 Motor, Door를 정의

    * 제조업체가 다른 경우, 구체적인 제어방식은 다르지만, 엘리베이터 입장에서는 모터를 구동해서 엘리베이터를 이동시킨다는 점은 동일
    * 그러므로 추상클래스로 Motor를 정의하고 여러 제조업체의 모터를 하위클래스로 정의할 수 있도록 한다.

  * Motor 클래스 - 연관 관계 -> Door

    * Motor 클래스는 이동하기 전에 문을 닫아야한다.
    * Door 객체의 getDoorStatus() 메서드를 호출하기 위해 연관 관계가 필요하다.

  * Motor 클래스의 핵심기능인 이동은 `move()` 메서드로 정의

    ```java
    public void move(Direction direction){
        // 1. 이미 이동중이면 무시
        // 2. 만약 문이 열려있다면, 문을 닫는다.
        // 3. 모터를 구동해서 이동시킨다. -> 제조업체에 따라 다르다.
        // 4. 모터의 상태를 이동 중으로 설정한다.
    }
    ```

    * 4단계의 동작 모두 각 제조업체 별 Motor 클래스의 공통 기능이고, 3번만 달라진다.
    * **템플릿 메서드 패턴 이용**
      * 즉, 일반적인 흐름에서는 동일하지만, 특정 부분만 다른 동작을 하는 경우에는 일반적인 기능을 상위클래스에 템플릿 메서드로서 설계할 수 있다.
      * 특정 부분에 대해서는 하위클래스에서 오버라이드 할 수 있도록 한다.

  * Door클래스의 `open()`, `close()` 메서드 정의

    ```java
    public void open(){
        // 1. 이미 문이 열려 있으면 무시
        // 2. 문을 닫는다 -> 제조 업체에 따라 다름
        // 3. 문의 상태를 '닫힘'으로 설정한다.
    }
    ```

    * 나머지 동작은 공통, 2번만 제조업체에 따라 달라진다.
    * 즉, `move()` 메서드와 같이 **템플릿 메서드 패턴** 을 적용할 수 있다.

* **Template Method**를 적용해서 구현한 코드

  *Door.class*

  ```java
  public abstract class Door {
      private DoorStatus doorStatus;
  
      public Door(){
          doorStatus = DoorStatus.CLOSED;
      }
  
      public DoorStatus getDoorStatus() {
          return doorStatus;
      }
  
      // primitive  or hook method
      protected abstract void doClose();
      // template method : close door
      public void close(){
          // if door is already closed, do nothing
          if(doorStatus == DoorStatus.CLOSED){
              return;
          }
  
          // primitive or hook method, Override from under class
          doClose(); // close the door
          doorStatus = DoorStatus.CLOSED; // record door status closed.
      }
  
      // primitive or hook method
      protected abstract void doOpen();
      // template method : open door
      public void open(){
          // if already door opened, do nothing
          if(doorStatus == DoorStatusd.OPENED){
              return;
          }
          doOpen(); // open the door
          doorStatus == DoorStatus.OPENED; // record door status opened
      }
  }
  ```

  *LGDoor.class*

  ```java
  public class LGDoor extends Door {
  
      @Override
      protected void doClose() {
          System.out.println("Close LG Door");
      }
  
      @Override
      protected void doOpen() {
          System.out.println("Open LG Door");
      }
  }
  ```

  *HyundaiDoor.class*

  ```java
  public class HyundaiDoor extends Door {
      @Override
      protected void doClose() {
          System.out.println("lotte door close");
      }
  
      @Override
      protected void doOpen() {
          System.out.println("lotte door open");
      }
  }
  ```

  * 엘리베이터 입장에서는 특정 제조 업체의 모터와 문을 제어하는 클래스가 필요
    * 예를 들어 LGMotor, HyundaiMotor 객체가 필요
    * **팩토리 메서드 패턴** 사용

* **팩토리 메서드 패턴** 을 적용한 예제 code

  ![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-example1.png)

  * 객체 생성 처리를 서브 클래스로 분리하여 캡슐화 함으로써 객체 생성의 변화에 대비할 수 있다.

  *VendorID.class*

  ```java
  public enum VendorID {
      LG, Hyundai
  }
  ```

  *MotorFactory.class*

  ```java
  public class MotorFactory {
      // vendorID에 따라서 LGMotor 또는 HyundaiMotor 객체를 생성
      public static Motor createMotor(VendorID vendorID){
          Motor motor = null;
  
          switch (vendorID){
              case LG:
                  motor = new LGMotor();
                  break;
              case Hyundai:
                  motor = new HyundaiMotor();
                  break;
          }
          return motor;
      }
  }
  ```

  * MotorFactory 클래스의 `createMotor()`메서드는 인자로 주어진 `Vendor ID`에 따라 LG Monitor 객체 또는 HyundaiMonitor 객체를 생성한다.

  *DoorFactory.class*

  ```java
  public class DoorFactory {
      // vendorID에 따라 LGDoor 또는 HyundaiDoor 객체를 생성
      public static Door createDoor(VendorID vendorID){
          Door door = null;
  
          switch (vendorID){
              case LG:
                  door = new LGDoor();
                  break;
              case Hyundai:
                  door = new HyundaiDoor();
                  break;
          }
          return door;
      }
  }
  ```

  * DoorFactory 클래스의 `createDoor()`메서드는 인자로 주어진 `Vendor ID`에 따라 LGDoor객체 또는 HyundaiDoor 객체를 생성.

  *Client.class*

  ```java
  public class Client {
      public static void main(String[] args) {
          Door lgDoor = DoorFactory.createDoor(VendorID.LG); // Call Factory Method
          Motor lgMotor = MotorFactory.createMotor(VendorID.LG); // Call Factory Method
          lgMotor.setDoor(lgDoor);
          lgDoor.open();
          lgMotor.move(Direction.UP);
      }
  }
  ```

  * 제조 업체에 따라 모터와 문에 해당하는 구체적인 클래스를 생성해야한다.
  * LGDoor와 LGMotor 객체를 활용하므로 `move()` 메서드를 호출하기 전 `open()` 에 의해 문이 열려있는 상태이다.
  * 그러므로 `move()` 메서드는 문을 먼저 닫고 이동을 시작한다.
  * 이때, LGDoor와 LGMotor객체가 모두 사용된다.

  ```text
  // 실행결과
  open LG Door
  close LG Door
  move LG Motor
  ```

### Issue

1. 다른 제조 업체의 부품을 사용해야하는 경우.
   * 기존의 부품 대신 다른 제조 업체의 부품을 사용해야한다면?
     * 총 10개의 부품을 사용해야한다면 각 Factory 클래스를 구현하고 이들의 Factory 객체를 각각 생성해야한다.
     * 즉, 부품의 수가 많아지면 특정 업체별 부품을 생성하는 코드의 길이가 길어지고 복잡해진다.
2. 새로운 제조 업체의 부품을 지원해야하는 경우
   * 새로운 제조 업체가 생길때마다 DoorFactory 연관된 Factory 클래스에서도 마친가지로 생성할 수 있도록 변경해야한다.
   * 또한 위와 마친가지로 특정 업체별 부품을 생성하는 코드에서 새로운 제조 업체의 부품을 생성하도록 모두 변경해야한다.

* 결과적으로 기존의 팩토리 메서드 패턴을 이용한 객체 생성은 관련 있는 여러 개의 객체를 일관성 있는 방식으로 생성하는 경우에 많은 코드 변경이 발생하게 되는 것이다.

### 해결책

**여러 종류의 객체를 생성할 때, 객체들 사이의 관련성이 있는 경우** 라면 각 종류별로 별도의 Factory 클래스를 사용하는 대신, 관련 객체들을 일관성 있게 생성하는 Factory 클래스를 사용하는 것이 좀 더 편할 수 있다.

![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-solution1.png)

* 예를 들어 MotorFactory, DoorFactory 클래스와 같이 *부품별로* Factory 클래스를 만드는 대신, `LGElevatorFactory`나 `HyundaiElevatorFactory` 클래스와 같이 *제조업체 별로* Factory 클래스를 만들 수도 있다.
* ~ElevatorFactory: ~Motor, ~Door 객체를 생성하는 Factory 클래스

*ElevatorFactory.java*

```java
/* 추상 부품을 생성하는 추상 팩토리 클래스 */
public abstract class ElevatorFactory {
  public abstract Motor createMotor();
  public abstract Door createDoor();
}
```

*LGElevatorFactory.java*

```java
/* LG 부품을 생성하는 팩토리 클래스 */
public class LGElevatorFactory extends ElevatorFactory {
  public Motor createMotor() { return new LGMotor(); }
  public Door createDoor() { return new LGDoor(); }
}
```

*Client.java*

```java
public class Client {
    public static void main(String[] args) {
        ElevatorFactory factory = null;
        String [] vendors = {"LG", "Hyundai"};
        String vendorName = vendors[0];

        // 인자에 따라 LG 또는 Hyundai 팩토리를 생성
        if(vendorName.equalsIgnoreCase("LG")){
            factory = new LGElevatorFactory();
        } else{
            factory = new HyundaiElevator();
        }

        Door door = factory.createDoor();
        Motor motor  = factory.createMotor();
        motor.setDoor(door);

        door.open();
        motor.move(Direction.UP);
    }
}
```

* 인자로 주어진 업체 이름에 따라서 적절한 부품 객체를 생성

  * 문제점 1과 같이 다른 제조 업체의 부품으로 변경하는 경우에도 Client 코드를 변경할 필요가 없다.

* 제조 업체별로 Factory 클래스를 정의했으므로, 제조 업체별 부품 객체를 간단하게 생성할 수 있다.

  * 즉, 문제점 2와 같이 새로운 제조 업체의 부품을 지원하는 경우에도 해당 제조 업체의 부품을 생성하는 Factory 클래스만 만들어주면 된다.

* UML

  ![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-solution2.png)

### 추가 보완(좀 더 완벽한 추상 팩토리 패턴)

![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-solution3.png)

1. "팩토리 메서드 패턴"

   * 제조 업체별 Factory 객체를 생성하는 방식을 캡슐화 한다.
   * `ElevatorFactoryFactory` 클래스 :  vendorID에 따라 해당 제조 업체의 Factory 객체를 생성
   * `ElevatorFactoryFactory` 클래스의 `getFactory()`메서드 : 팩토리 메서드

   *ElevatorFactoryFactory.java*

   ```java
   public class ElevatorFactoryFactory {
       public static ElevatorFactory getFactory(VendorID vendorID){
           ElevatorFactory factory = null;
   
           switch (vendorID){
               case LG:
                   factory = new LGElevatorFactory().getInstance();
                   break;
               case Hyundai:
                   factory = new HyundaiElevator().getInstance();
                   break;
           }
           return factory;
       }
   }
   ```

2. 싱글턴 패턴 

   * 제조 업체 별 Factory 객체는 각각 1개만 있으면 된다.
   * 2개의 제조 업체별 Factory 클래스를 싱글턴으로 설계

   *LGElevatorFactory.java*

   ```java
   /* LG 부품을 생성하는 팩토리 클래스 */
   public class LGElevatorFactory extends ElevatorFactory {
       private static ElevatorFactory factory;
   
       protected LGElevatorFactory() {
       }
   
   
       @Override
       public Motor createMotor() {
           return new LGMotor();
       }
   
       @Override
       public Door createDoor() {
           return new LGDoor();
       }
   
       @Override
       public ElevatorFactory getInstance() {
           if(factory == null)
               factory = new LGElevatorFactory();
   
           return factory;
       }
   }
   ```

*Client.java*

```java
public class Client {
    public static void main(String[] args) {
        String vendorName = "LG";
        VendorID vendorID;

        // 인자에 따라 LG 또는 Hyundai 팩토리를 생성
        if(vendorName.equalsIgnoreCase("LG")){
            vendorID = VendorID.LG;
        } else{
            vendorID = VendorID.Hyundai;
        }

        ElevatorFactory factory = ElevatorFactoryFactory.getFactory(vendorID);

        Door door = factory.createDoor();
        Motor motor  = factory.createMotor();
        motor.setDoor(door);

        door.open();
        motor.move(Direction.UP);
    }
}
```

### 추상 팩토리 패턴 최종 클래스 다이어그램

![](https://gmlwjd9405.github.io/images/design-pattern-abstract-factory/abstract-factory-solution4.png)

* “AbstractFactory”: ElevatorFactory 클래스
* “ConcreteFactory”: LGElevatorFactory 클래스와 HyundaiElevatorFactory 클래스
* “AbstractProductA”: Motor 클래스
* “ConcreteProductA”: LGMotor 클래스와 HyundaiMotor 클래스
* “AbstractProductB”: Door 클래스
* “ConcreteProductB”: LGDoor 클래스와 HyundaiDoor 클래스
