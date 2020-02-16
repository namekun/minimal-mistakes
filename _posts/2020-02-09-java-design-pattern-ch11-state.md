---
title: "자바 디자인패턴 State pattern"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## State **패턴**

### 형광등 만들기

형광등의 로직은 간단. 버튼을 눌렀을 때 꺼져있으면 켜지고, 켜져있으면 꺼진다.

*Light_BeforeState.java*

```java
package ch11_state.beforeState;

/*
 * state 패턴을 적용하기 전 step1
 */
public class Light_beforeState {
    // 형광등 상태를 표현하는 상수
    private static int ON = 0;
    private static int OFF = 1;

    private int state;

    public Light_beforeState(){
        state = OFF; // 형광등 초기 상태는 꺼져있음
    }

    public void on_button_pushed(){
        if(state== ON){
            System.out.println("반응 없음"); // 전구가 이미 켜진 상태라면, 반응 없음
        } else {
            System.out.println("light on!"); // 전구가 꺼져있으면 반응
            state = ON;
        }
    }

    public void off_button_pushed(){
        if(state == OFF){
            System.out.println("반응 없음");
        } else {
            System.out.println("light off!");
            state = OFF;
        }
    }
}
```



*Client_BeforeState.java*

```java
public class Client_beforeState {
    public static void main(String[] args) {
        Light_beforeState light = new Light_beforeState();
        light.off_button_pushed();
        light.on_button_pushed();
        light.on_button_pushed();
        light.off_button_pushed();
    }
}
```



### 문제점

형광등에 '취침등'을 추가해보자

요구사항은 다음과 같다.

* 형광등이 켜져있을 때, On버튼을 누르면, 원래는 켜진 상태 그대로 였지만, 지금은 '취침등'상태로 변경. 그 상태에서, 버튼을 한번 더 눌러야 Off 상태로 간다.

* 우선 취침등 상태를 추가한다.

  ```java
  private static int SLEEPING = 2;
  ```

* 그 다음 `on_button_pushed`상태를 바꿔야 한다.

  ```java
   public void on_button_pushed(){
          if(state== ON){
      		System.out.println("취침등");
               state = SLEEPING;
          } else if(state == SLEEPING) {
              System.out.println("light on!"); // 전구가 꺼져있으면 반응
              state = ON;
          } else {
              System.out.println("light on!");
              state = ON;
          }
      }
  ```

* 최종코드는 다음과 같아진다.

  ```java
  package ch11_state.step2;
  
  /*
   * state 패턴을 적용하기 전 step2
   */
  public class Light_beforeState {
      // 형광등 상태를 표현하는 상수
      private static int ON = 0;
      private static int OFF = 1;
      private static int SLEEPING = 2;
  
      private int state;
  
      public Light_beforeState() {
          state = OFF; // 형광등 초기 상태는 꺼져있음
      }
  
      public void on_button_pushed() {
          if (state == ON) {
              System.out.println("취침등");
              state = SLEEPING;
          } else if (state == SLEEPING) {
              System.out.println("light on!"); // 전구가 꺼져있으면 반응
              state = ON;
          } else {
              System.out.println("light on!");
              state = ON;
          }
      }
  
      public void off_button_pushed() {
          if (state == OFF) {
              System.out.println("반응 없음");
          } else if (state == SLEEPING) {
              System.out.println("Light off");
              state = OFF;
          } else {
              System.out.println("light off!");
              state = OFF;
          }
      }
  }
  ```

### state pattern 적용

state 패턴을 적용해보자.

목표는 현재 시스템이 어떤 상태에 있는지와 상관없이 구성하고, 상태변화에도 독립적일 수 있도록 코드를 수정하는 것이다.

이를 위해서는 상태를 클래스로 분리해 캡슐화 할 수 있도록 한다. 또한 상태에 의존적인 행위들도 상태 클래스에 같이 두어 특정 상태에 따른 행위를 구현하도록 바꾼다. 이렇게 하면 상태에 따른 행위가 각 클래스에 국지화 되어 이해하고 수정하기가 쉽다.

*UML*

![](/_data/state.png)

Light 클래스의 state 변수를 통해 현재 시스템의 상태 객체를 참조한다. 상태에 따른 행위를 수행하려면, state 변수가 참조하는 상태 객체에 작업을 위임해야한다.

Light클래스 코드 어디를 보더라도, 구체적인 상태를 나타내는 객체를 찹조하지 않는다. 즉, 시스템이 어떻든 전혀 영향을 받지 않는다는 의미이다.

### 최종 코드

*State.interface*

```java
public interface State {
    public void on_button_pushed(Light light);
    public void off_button_pushed(Light light);
}
```

*Light.java*

```java
public class Light {
    private State state;
    public Light(){
        state = new OFF();
    }

    public void setState(State state){
        this.state = state;
    }

    public void on_button_pushed(){
        state.on_button_pushed(this);
    }

    public void off_button_pushed(){
        state.off_button_pushed(this);
    }
}
```

*Off.java*

```java
public class OFF implements State{
    private static OFF off = new OFF(); // OFF 클래스의 인스턴스로 초기화
    protected OFF(){}

    public static OFF getInstance(){
        return off;
    }

    @Override
    public void on_button_pushed(Light light) {
        light.setState(ON.getInstance());
        System.out.println("Light On");
    }

    @Override
    public void off_button_pushed(Light light) {
        System.out.println("반응 없음");
    }
}
```

*ON.java*

```java
public class ON implements State{
    private static ON on = new ON(); // ON 클래스의 인스턴스로 초기화
    private ON(){}

    protected static ON getInstance(){ // 초기화된 ON 클래스의 인스턴스를 반환
        return on;
    }

    @Override
    public void on_button_pushed(Light light) {
        System.out.println("반응 없음");
    }

    @Override
    public void off_button_pushed(Light light) {
        light.setState(OFF.getInstance());
        System.out.println("Light Off");
    }
}
```



### STATE

state 패턴은 어떤 행위를 수행할 때, 상태에 행위를 수행하도록 위임한다. 이를 위해 스테이트 패턴에서는 시느템의 각 상태를 클래스로 분리해 표현하고, 각 클래스에서 수행하는 행위들을 메서드로 구현한다. 그리고 이러한 상태들을 외부로부터 캡슐화하기 위해 인터페이스를 만들어 시스템의 각 상태를 나타내는 클래스로 하여금 실체화 하게 한다.

![](https://upload.wikimedia.org/wikipedia/de/7/70/StatePattern_Classdiagramm.png)

* state : 시스템의 모든 상태에 공통의 인터페이스를 제공한다. 따라서 이 인터페이스를 실체화한 어떤 상태 클래스도 기존 상태클래스를 대신해 교체해서 사용할 수 있다.
* state1, 2, 3 : Context 객체가 요청한 작업을 자신의 방식으로 실제 실행한다. 대부분의 경우, 다음 상태를 결정해 상태 변경을 Context 객체에 요청하는 역할도 수행한다.
* context : state를 이용하는 역할을 수행한다. 현재 시슽메의 상태를 나타내는 상태 변수(state)와 실제 시스템의 상태를 구성하는 여러가지 변수가 잇다. 또한 각 상태 클래스 에서 상태 변경을 요청해 상태를 바꿀 수 있도록 하는 메서드(`setState`)가 제공된다. Context 요소를 구현한 클래스의 request 메서드는 실제 행위를 실행하는 대신, 해당 상태 객체에 행위 실행을 위임한다.