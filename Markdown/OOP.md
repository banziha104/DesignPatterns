# 프로그래밍

> 소프트웨어를 구현한다는 것을 소프트웨어를 구성하는 데이터와 데이터를 조작하는 코드를 만듬

- 절차지향 : 데이터를 조작하는 코드를 별도로 분리해서 함수나 프로시저와 같은 형태로만들고 프로시저는 다른 프로시저를 사용할 수 도 있으며 각각의 프로시저가 같은 데이터를 사용할 수도 있음
- 객체지향 : 데이터와 데이터와 관련된 프로시저를 객체라는 단위로 묶어서 프로그램을 구성함

---

<br>

# 객체지향

- 객체의 핵심은 기능을 제공하는 것. 
- 오퍼레이션 : 객체가 제공하는 기능
- 시그니쳐 : 기능 식별 이름 + 파라미터 및 파라미터 타 + 기능 실행 결과 값
- 인터페이스 : 객체가 제공하는 모든 오퍼레이션 집합
- 메세지 : 오퍼레이션의 실행을 요청하는 것
- 의존 : 한 객체가 다른 객체를 생성하거나 다른 객체의 메서드를 호출할 때, 이를 해당 객체에 대한 의존한다라고 표현함.
- 캡슐화 : 객체가 내부적으로 어떻게 구현하는지 감추는 것, 내부 구현 변경의 유연성 획득.

```java
public class Member{
    private Date expiryDate;
    
    /* 만료 여부 확인을 구현을 캡슐화*/
    
    public boolean isExpired(){
        return expiryDate;
    }
}
```

- Tell, Don't Ask : 캡슐화의 첫 번쨰 규칙으로 데이터를 물어보지 않고 기능을 실행해 달라고 말하는 규칙, 데이터 대신에 기능으로 대체함
- 데미테르의 법칙 
    - 메서드에서 생성한 객체의 메서드만 호출
    - 파라미터로 받은 객체의 메서드만 호출
    - 필드로 참조하는 객체의 메서드만 호출
 
```java
if(member.getDate().getTime()){ // 데미테르 법칙 위반, Member 객체의 메소드에 Date 메소드를 동시에 사용.
    
}
```

# 다형성과 추상 타입

- 다형성 : 한 객체가 여러 가지의 모습을 갖는다는 것을 의미
- 구현 상속 : 클래스 상속을 통해서 이루어지며 보통 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용됨
- Mock 객체 : 실제 클래스 대신에 신짜처럼 행동하는 객체를 뜻함.
- 추상화 : 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의함.

```java
interface LogCollector{
    void collect();
} 

LogCollector collector = createLogCollector();
collector.collect(); // 상속받은 클래스의 collect가 실행됌.

```

- 추상화의 유연함

```java
/*추상화하지 않는 예제, if elss 블락이 계속 늘어나며, 본연의 책임(흐름제어)와 상관없는 데이터 읽기도 변경해야함*/

if(useFile()){
    FiledataRedaer fileReader = new FileDataReader();
    data = fileReader.read();
}else{
    SocketDataReader socketReader = new SockerReader();
    data = socketReader.read();
}

```

```java
/*추상화 한 예제*/
public interface ByteSource{
    byte[] read();
}

public class FileDataReader implements ByteSource{
    //read 구현
}

public class SocketDataReader implements ByteSource{
    //read 구현
}
```

```java
ByteSource source =null;
if(useFile) source = new FileDataReader();
else source = new SocketDataReader();
byte[] data = source.read();
```

<br>

---
