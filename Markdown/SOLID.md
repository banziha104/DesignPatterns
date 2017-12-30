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


