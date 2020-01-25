---
title: "자바 디자인패턴 Factory-Method"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## Java Design Pattern 4th Class - Factory Method


### 구조

---

![https://johngrib.github.io/post-img/factory-method-pattern/structure.gif](https://johngrib.github.io/post-img/factory-method-pattern/structure.gif)

### 요약

---

- 객체 생성을 캡슐화 하는 페턴
- 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스가 내리도록 한다.

### 구현시에 고려해야할 점들

---

- 팩토리 메서드 패턴의 구현방법에는 크게 두가지가 있다.
    - Creator를 추상 클래스로 정의하고, 팩토리 메서드는 abstract로 선언하는 방법
    - Creator가 구체 클래스이고, 팩토리 메소드의 기본 구현을 제공하는 방법

- 팩토리 메서드의 인자를 통해 다양한 product를 생성하게 한다.
    - 팩토리 메서드에 잘못된 인자가 들어올 경우 런타임 에러 처리에 대해 고민
    - Enum 등을 사용하는 것도 고려할 필요가 있다.
- 프로그래밍 언어별로 구현방법에 차이가 있다.

### 활용성

---

팩토리 메서드는 다음과 같은 상황에서 사용

- 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때
- 객체 생성의 책임을 몇 개의 보조 서브 클래스 가운데 하나에게 위임하고, 어떤 서브 클래스가 위임자인지에 대한 정보를 국소화 시키고 싶을 때.

### 코드

---

*Product.java*
```java
    public abstract class Product {
        public abstract void use();
    }
```
*Factory.java*
```java
    public abstract class Factory {
        public final Product create(String owner){
            Product p = createProduct(owner);
            registerProduct(p);
            return p;
        }
    
        protected abstract Product createProduct(String owner);
        protected abstract void registerProduct(Product p);
    
    }
```
*IDCard.java*
```java
    public class IDCard extends Product {
        private String owner;
    
        public IDCard(String owner){
            System.out.println(owner + "의 카드를 만듭니다.");
            this.owner = owner;
        }
    
        @Override
        public void use() {
            System.out.println(owner + "의 카드를 사용합니다.");
        }
    
        public String getOwner() {
            return owner;
        }
    }
```
*IDCardFactory.java*
```java
    public class IDCardFactory extends Factory {
        private List<String> owners = new ArrayList<String>();
    
        @Override
        protected Product createProduct(String owner){ // Factory Method
            return new IDCard(owner);
        }
    
        @Override
        protected void registerProduct(Product p) {
            owners.add(((IDCard) p).getOwner());
        }
    
        public List<String> getOwners(){
            return owners;
        }
    }
```
*Main.java*
```java
    public class Main {
        public static void main(String[] args) {
            Factory factory = new IDCardFactory();
            Product card1 = factory.create("이순신");
            Product card2 = factory.create("장보고");
            Product card3 = factory.create("아무개");
    
            card1.use();
            card2.use();
            card3.use();
        }
    }
```
### 결과

---
```
    이순신의 카드를 만듭니다.
    장보고의 카드를 만듭니다.
    아무개의 카드를 만듭니다.
    이순신의 카드를 사용합니다.
    장보고의 카드를 사용합니다.
    아무개의 카드를 사용합니다.
```
