# 설계원칙 : SOLID

1. 단일 책임의 원칙 (Single responsibility principle)

> 클래스는 단 한개의 책임을 가져야 한다. 

- 클래스가 여러 책임을 갖게 되면, 그 클래스는 각 책임마다 변경되는 이유가 발생하기 때문에 한개의 책임만 가져야한다
- 즉 클래스를 변경하는 이유는 단 한개여야한다.
- 단일 책임의 원칙을 지키기 위해선, 메소드의 사용자를 파악하면 편함.
- 클래스의 사용자들이 서로 다른 메서드를 사용한다면 , 책임 분리 후보가 될 수 있음.

```java
// 잘못된 예, 데이터를 읽거나 화면에 보여주는 클래스가 바뀌는 경우 같이 바뀌어야함.
public class DataViewer{
    public void display(){
        String data = loadHtml();
        updateGui(data);
    }
    
    /*데이터를 읽는 책임*/
    public String loadHtml(){
        HttpClient client = new HttpClient();
        client.connetc();
        return client;
    }
    /*화면에 보여주는 책임*/
    public void updateGui(String data){
        GuiData guiModel = parseDataoGuiData(data);
        tableUI.changeData(guiModel);
    }
}
```

2. 개방 폐쇄의 원칙 (Open-closed principle)

> 확장에는 열려있어야하고, 변경에는 닫혀있어야한다. 기능을 변경하거나 확장할 수 있으면서 그 기능을 사용하는 코드는 수정하지 않는다.

- 인터페이스와 상속을 통해 실현한다
- 다운캐스팅, instanceof, 비슷한 if-else 블록이 존재하면, 개방폐쇄의 원칙이 깨졌을 확률이 높다.
- 개방 폐쇄 원칙은 유연함에 대한 것이다.

```java
public class ResponseSender{
    private Data data;
    public ResponseSender(Data data){
        this.data = data;
    }
    
    public Data getData(){
        return data;
    }
    
    public void send(){
        sendHeader();
        sendBody();
    }
    
    protected void sendHeader(){
        // 헤더 데이터 전송
    }
    
    protected void sendBody(){
        // 바디 데이터 전송
    }
}
```

```java
/* 압축전송하는 기능 추가 
* 새로운 클래스는 기존 기능에 압축 기능을 추가해 주는데, 이를 위해 ResponseSender 클래스의 코드는 바뀌지 않았음.
* */

public class ZippedResponseSender extends ResponseSender{
    public ZippedResponseSender(Data data){
        super(data);
    }
    
    @Override
    protected void sendBody(){
        // 데이터 압축처리
    }
}
```

3. 리스코프 치환 원칙(Liskov substitution principle)

> 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야한다.

- 명시된 명세에서 벗어난 값을 리턴, 익셉션을 발생, 기능을 수행하면 위반
- 리스코프 치환 원칙은 계약(기능 명세)과 확장에 관한것. 
- 개방 폐쇄의 원칙과 밀접한 관련을 가짐

```java
public class Rectangle {
    private int width;
    private int height;
    
    public void setWidth(int width){
        this.width = width;
    }
    public void setHeight(int height){
        this.height = height;
    }
    public void getWidth(){
        return width;
    }
    public void getHeight(){
        return height;
    }
}

/* 잘못된 예, Rectangle의 setWidth와 setHeight는 각각 높이값, 폭값만 변경된다고 명세되어있지만,
* 아래의 경우에는 두가지를 함*/

public class Square extends Rectangle{
    @Override
    public void setWidth(int width){
        super.setWidth(width);
        super.setHeight(width);
    }
    @Override
    public void setHeight(int height){
        super.setWidth(height);
        super.setHeight(height);
    }
        
}

public class Main{
    /*이 경우 square로 다운캐스팅하게 되면, 문제가 생김*/
    public void increseHeight(Rectangle rectangle){
        rectangle.setHeight(rectangle.setWidth()+10);
    }
}
```

4. 인터페이스 분리 원칙 (Interface segregation principle)

> 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야한다.

- 인터페이스를 각 클라이언트가 필요로 하는 인터페이스들로 분리함으로써, 각 클라이언트가 사용하지 않는 인터페이스에 변경이 발생하더라도, 영향을 받지 않도록 하여아함
- 인터페이스 분리의 원칙은 클라이언트에 대한 것

5. 의존 역전의 원칙 (Dependency inversion principle)

> 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안되며 저수준의 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야한다.

- 고수준 모듈 : 의미 있는 단일 기능을 제공하는 모듈
- 저수준 모듈 : 고수준의 모듈의 기능을 구현하기 위해 필요한 하위 기능을 실제로 구현 

- 예
    - 고수준 모듈 : 바이트 데이터를 읽어와 암호화 하고 결과 바이트 데이터를 쓴다
    - 저수준 모듈 : 
        1. 파일에서 바이트 데이터를 읽어온다
        2. 알고리즘으로 암호화한다
        3. 파일에 바이트 데이터를 쓴다.
        
- 의존 역전 원칙을 통한 변경의 유연함을 확보하기 위함.


```java
/*올바르지 않은 예
* FlowContoller가 하위 객체인 FileDataReader에 의존함*/
public class FlowController{
    public void process(){
        FileDataReader reader = new FileDataReader(); // 의존 발생
    }
}
```

```java
/* 올바른 예
 * FileDataReader 클래스의 소스코드가 추상화 타입인 ByteSource에 의존하게 됌 */
public class FileDataReader implements ByteSoruce{
    
}
```