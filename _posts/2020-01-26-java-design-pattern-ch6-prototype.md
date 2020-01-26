---
title: "자바 디자인패턴 Proto type"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## Prototype Pattern

* 용도
  * 미리 만들어진 객체를 복사해서 객체를 생성하는 방식
  * 객체를 많이 만들어야하는 경우, 객체 생성에 드는 코딩 분량을 현저히 줄일 수 있다.
  * 클래스로부터 객체를 생성하기 어려운 경우에 사용
* 사용 방법
  * 모형(proto type) 인스턴스를 등록해놓은 뒤, 등록된 인스턴스를 복사(clone)해서 인스턴스를 생성함.
* class diagram

![](https://realzero0.github.io/assets/img/prototypepattern.png)

### code

*Users.java*

```java
import java.util.ArrayList;
import java.util.List;

public class Users  implements Cloneable{
    private List<String> userlist;

    public Users(List<String> userlist) {
        this.userlist = userlist;
    }

    public Users() {
        userlist = new ArrayList<>();
    }

    public void loadData(){
        userlist.add("abc");
        userlist.add("vds");
        userlist.add("cdg");
        userlist.add("hby");
    }

    public List<String> getUserlist(){
        return userlist;
    }

    @Override
    public Object clone() throws CloneNotSupportedException{
        List temp = new ArrayList();
        for(String s : this.getUserlist()){
            temp.add(s);
        }
        return new Users(temp);
    }
}
```

*Main.java*

```java
import java.util.List;

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {

        Users originUsers = new Users();
        originUsers.loadData();

        Users cloneUsers = (Users) originUsers.clone();

        List<String> origin = originUsers.getUserlist();

        for(String user : origin){
            System.out.println(user);
        }

        List<String> ex = cloneUsers.getUserlist();

        for(String user : ex){
            System.out.println(user); // clone이라 내용은 똑같다.
        }
    }
}
```
