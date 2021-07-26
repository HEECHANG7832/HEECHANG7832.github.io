---
layout: post
title: "디자인 패턴"
categories:
  - Spring & SpringBoot
tags:
  - Spring & SpringBoot
---

### Singleton Pattern

- 어떠한 객체가 유일하게 1개만 존재할때
- TCP Socket 통신에서 서버와 연결된 connect 객체에 주로 사용한다



```java

public class SocketClient{
  private SocketClient socketClient;

  private SocketClient(){}

  public static SocketClient getInstance(){
    if(socketClient == null){
      socketClient = new SocketClient();
    }
    return socketClient;
  }

  public void connect(){
    System.out.println("connect");
  }
}

```


### Adapter Pattern
- 호환성이 없는 기존 클래스의 인터페이스를 변환하여 재사용 할 수 있게 한다

```java
public interface Electronic110V{
  void powerOn();
}
public interface Electronic220V{
  void connect();
}

public class HairDryer implements Electronic110V{
  @Override
  public void powerOn(){
    System.out.println("HairDryer");
  }
}
public class AirConditioner implements Electronic220V{
  @Override
  public void powerOn(){
    System.out.println("AirConditioner");
  }
}

public class SocketAdapter implements Electronic110V{
  private Electronic220V electronic220V;

  public SocketAdapter(Electronic220V electronic220V){
    this.electronic220V = electronic220V;
  }

  @Override
  public void powerON(){
    electronic220V.connect();
  }
}
public class Main{
  public static void main(String[] args){
    HairDryer hairDryer = new HairDryer();
    connect(hairDryer); //"HairDryer"

    AirConditioner airconditioner = new AirConditioner();

    //Adapter!!!
    Electronic110V adapter = new SocketAdapter(aircontitioner);
    connect(adapter); //사용하는건 powerON()으로 동일
  }

  public static void connect(Electronic110V electronic110V){
    electronic110V.powerOn();
  }
}

```
