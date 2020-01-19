---
title: "java-design-pattern template Method"
tags:
  - Java
  - designPattern
  - gof
comments: true
---

## 3. Template Method Pattern

- 공통적인 프로세스를 묶어줄 수 있다.
- 알고리즘의 구조를 메서드에 정의하고, 하위클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의하는 패턴

### 어떨때 사용할까?

- 구현하고자 하는 알고리즘이 일정한 프로세스가 있다.
- 구현하려는 알고리즘이 변경 가능성이 있다.

### 어떻게 사용할까

- 알고리즘을 여러 단계로 나눈다.
- 나눠진 알고리즘의 단계를 메서드로 선언
- 알고리즘을 수행할 템플릿 메서드를 만든다.
- 하위 클래스에서 나눠진 메서드들을 구현한다.

### 요구사항

- 신작 게임의 접속을 구현해주세요
    - requestConnection(String str) : String
- 유저가 게임 접속시 다음을 고려해야합니다.
    - 보안 과정 :  보안관련 부분을 처리해야합니다.
        - doSecurity(String string) : String
    - 인증 과정 : user name 과 password가 일치하는지 확인합니다.
        - authentication(String id, String Password) : boolean
    - 권한 과정 : 접속자가 유료회원인지 무료회원인지 게임 마스터 인지 확인합니다.
        - authorization(String userName) : int
    - 접속 과정 : 접속자에게 커넥션 정보를 넘겨줍니다.
        - connection(String info) : String

*AbstractGameConnectionHelper.java*
```java
    public abstract class AbstractGameConnectHelper {
    
        // 하위 클래스에서 재정의를 해야하기에 private 는 안됩니다.
        // 외부에서 호출은 막고, 하위 클래스에서 사용할 수 있도록 하는 
        // protected 를 사용해야한다.
        protected abstract String doSecurity(String string);
    
        protected abstract boolean authentication(String id, String password);
    
        protected abstract int authorization(String UserName);
    
        protected abstract String connection(String info);
    
        // Template Method
        public String requestConnection(String encodedInfo) {
            // 보안 과정
            String decodedInfo = doSecurity(encodedInfo);
    
            // 반환된 것을 갖고, 아이디, 암호를 할당한다.
            String id = "aaa";
            String pw = "bbb";
    
            if (!authentication(id, pw)){
                throw new Error("아이디, 또는 비밀번호를 확인해주세요");
            }
    
            String userName = "userName";
            int permission = authorization(userName);
    
            switch (permission){
                case 0:
                    System.out.println("Game_Manager");
                    break;
                case 1:
                    System.out.println("premier_User");
                    break;
                case 2:
                    System.out.println("norm_User");
                    break;
                case 3:
                    System.out.println("non-Permission");
                    break;
                default:
                    System.out.println("for expand");
                    break;
            }
    
    
            return connection(decodedInfo);
        }
    }
```
*DefaultGameConnectHelper*
```java
    public class DefaultGameConnection 
    		extends AbstractGameConnectHelper {
        @Override
        protected String doSecurity(String string) {
            System.out.println("decoding!");
            return string;
        }
    
        @Override
        protected boolean authentication(String id, String password)
    		{
            System.out.println("authentication!");
            return true; // if correct, return true
        }
    
        @Override
        protected int authorization(String UserName) {
            System.out.println("permission authorization!");
            return 0;
        }
    
        @Override
        protected String connection(String info) {
            System.out.println("the last connection phase!");
            return info;
        }
    }
```
*Main.java*
```java
    import ch3_TemplateMethod.dp.AbstractGameConnectHelper;
    import ch3_TemplateMethod.dp.DefaultGameConnection;
    
    public class Main {
        public static void main(String[] args) {
            AbstractGameConnectHelper helper = 
    				new DefaultGameConnection();
            helper.requestConnection("information");
        }
    }
```
### 추가 요구사항

- 보안 부분이 정부 정책에 의해 강화되었습니다.
- 강화된 방식으로 코드를 변경해야합니다.
- 여가부에서 밤 10시 이후로 접속이 제한되도록 합니다.

아주 간단하다. 어차피 뭐..우리가 직접적인 기능은 구현하는게 아니니깐!

*AbstractGameConnectHelper.java*
```java
    ...
    
    //        String userName = "userName";
    //        int permission = authorization(userName);
            String student = "student";
            int permission = authorization(student);
    
    
            switch (permission){
                case -1:
                    System.out.println("Computer Shut Down because of your Age");
                    break;
                case 0:
                    System.out.println("Game_Manager");
                    break;
                case 1:
                    System.out.println("premier_User");
                    break;
                case 2:
                    System.out.println("norm_User");
                    break;
                case 3:
                    System.out.println("non-Permission");
                    break;
                default:
                    System.out.println("for expand");
                    break;
            }
    
    
            return connection(decodedInfo);
        }
    
    ...
```
*DefaultGameConnection.java*
```java
    ...
    
    @Override
        protected int authorization(String UserName) {
            System.out.println("permission authorization!");
            // 서버에서 유저 이름, 나이를 알 수 있다.
            // 나이를 확인하고, 시간을 확인해서 미성년자, 
    			  // 그리고 저녁 10시가 지났다면
            // 바로 권한을 없는 것으로 처리해준다.
            if(UserName == "student"){
                return -1;
            }
            return 0;
        }
    
    
    ...
```
