---
title: "자바 디자인패턴 Builder"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## Builder Pattern

 ### 요약

* 복잡한 객체의 생성과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현결과를 만들 수 있도록 하는 패턴
* 선택적인 parameter가 많을 경우, 제공 상태를 일관성 있게 해주고, object를 생성시킬 때, step-by-step으로 만들 수 있도록 제공해주며, 최종에는 만들어진 object를 리턴.

### 작업순서

* class 안에 중첩 static class를 생성시키고, 바깥족 class의 argument들을 안쪽 static class(Builder class)로 옮긴다. 바깥쪽 class의 생성자는 private 로 선언하여 직접적인 생성을 막는다.
* builder class의 생성자를 public static 으로 선언하고, 필요한 parameter들을 요청한다.
* builder class에는 선택적 parameter에 대한 setter method가 있어야 한다. 그리고 선택적 parameter를 설정한 후에도 같은 builder 객체를 리턴해야한다.
* 마지막으로 클라이언트 프로그램이 요청하는 객체를 받을 수 있도록 build Method 를 만드는 데, build method에서는 바깥쪽 class의 생성자가 buiilder 클래스의 인자를 받을 수 있도록 제공한다.

### diagram

![](http://www.modelit.xyz/wp-content/uploads/2017/05/Builder2.png)

### Code

*Product.java*

```java
public class Product {

    // parameters
    private String name;
    private int price;

    // optional parameter
    private boolean isSell;

    public String getName() {
        return this.name;
    }

    public int getPrice() {
        return this.price;
    }

    public boolean isSellEnabled() {
        return isSell;
    }

    // argument -> ProductBuilder instance
    private Product(ProductBuilder builder) {
        this.name = builder.name;
        this.price = builder.price;
        this.isSell = builder.isSell;
    }

    public static class ProductBuilder {
        private String name;
        private int price;
        private boolean isSell;

        public ProductBuilder(String name, int price) {
            this.name = name;
            this.price = price;
        }

        public ProductBuilder setIsSellEnabled(boolean isSell) {
            this.isSell = isSell;
            return this;
        }

        public Product build() {
            return new Product(this);
        }
    }
}
```

*Main.java*

```java
public class Main {
    public static void main(String[] args) {
        Product p1 = new Product.ProductBuilder("test",1000).setIsSellEnabled(true).build();
        Product p2 = new Product.ProductBuilder("test2",10000).setIsSellEnabled(false).build();

    }
}
```
