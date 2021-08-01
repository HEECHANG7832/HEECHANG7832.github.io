---
layout: post
title: "디자인 패턴3"
categories:
  - SpringBoot
tags:
  - SpringBoot
---

### Observer pattern

- 변화가 일어났을때 미리 등록된 다른 클래스에 통보해주는 패턴
- event listener에서 해당 패턴을 사용 하고 있다

```java
public interface IButtonListener{
  void clickEvent(String event);
}

public class Button{
  private String name;
  private IButtonListener buttonListener;

  //Constructor
  ///

  public void click(String message){
    buttonListner.clickEvent(message);
  }
  public void addListener(IButtonListener buttonListener){
    this.buttonListener = buttonListener;
  }
}

public class Main{
  public static void main(String[] args){
      Button button = new Button("");
      button.addListener({
        @Override
        public void clickEvent(String event){
          System.out.println(event);
        }
      });

      button.click("click");
  }
}

```


### Facade Pattern
- 여러 객체와 실제 사용하는 서브 객체의 복잡한 의존관계가 있을때, facade라는 객체를 두고 interface만을 활용하여 기능을 사용하는 방식

```java
public class Ftp{
  //Constructor
  //connect(), moveDirectory(), disConnect()
}
public class Writer{
  //Constructor
  //fileConnect(), fileRead(), fileDisconnect()
}
public class Reader{
  //Constructor
  //fileConnect(), fileRead(), fileDisconnect()
}

public class SftpClient{
  private Ftp ftp;
  private Reader reader;
  private Writer writer;

  //Constructor

  public SftpClient(String host, int port, String path, String fileName){
    this.ftp = new Ftp(host.port. path);
    this.reader = new Reader(fileName);
    this.writer = new Writer(filename);
  }

  public void connect(){
    ftp.connect();
    ftp.moveDirectory();
    writer.fileConnect();
    reader.fileConnect();
  }

  public void disConnect(){
    writer.fileDisconnect();
    reader.fileDisconnect();
    ftp.disConnect();
  }

  public void read(){
    reader.fileRead();
  }

  public void write(){
    writer.write();
  }
}

public class Main{
  public static void main(String[] args){
    SftpClient sftpClinet = new SftpClient("naver.com", 22, "/home/etc", "test.txt");

    sftpClient.connect();
    sftpClient.write();
    sftpClient.read();
    sftpCleint.disConnect();
  }
}
```


### Strategy Pattern
- 유사한 행위들을 캠슐화하여 객체의 행위를 바꾸고 싶은 경우 객체를 수정하는게 아니라 전략만 수정하여 행위를 변경

```java
public interface EncodingStrategy{
  String encode(String text);
}

public class NormalStrategy implements EncodingStrategy{
  //@Override
  //개별 전략 구현
}
public class Base64Strategy implements EncodingStrategy{
  //@Override
  //개별 전략 구현
}
public class Encoder{
  private EncodingStrategy encodingStrategy;
  //EncodingStrategy setter

  public String getMessage(String message){
    return encodingStrategy.encode(message);
  }
}

public class Main{
  public static void main(String[] args){
      EncodingStrategy base64 = new Base64Strategy();
      EncodingStrategy normal = new NormalStrategy();

      String message = "hello java";

      encoder.setEncodingStrategy(base64);
      System.out.println(encoder.getMessage(message));

      encoder.setEncodingStrategy(normal); //전략 변경
      System.out.println(encoder.getMessage(message));
  }
}
```
