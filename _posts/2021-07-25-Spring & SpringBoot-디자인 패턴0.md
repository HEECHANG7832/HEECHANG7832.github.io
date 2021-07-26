---
layout: post
title: "디자인 패턴2"
categories:
  - Spring & SpringBoot
tags:
  - Spring & SpringBoot
---

### Proxy Pattern
- 대신해서 처리하는 것
- Client는 Proxy로 부터 결과를 받는다
- Cache의 기능으로 활용 가능
- 개방폐쇄 원칙, 의존 역전 원칙
- Spring AOP에서 사용

```java
public interface IBrowser{
  Html show();
}

public class Html{
  private String url;

  public Html(String url){
    this.url = url;
  }
}

public class Browser implements IBrowser{
  private String url;
  public Browser(String url){
    this.url = url;
  }
  @Override
  public Html show(){
    System.out.println(url);
    return url;
  }
}
public class BrowserProxy implements IBrowser{
  private String url;
  private Html html;

  public BrowserProxy(String url){
    this.url = url;
  }

  @Override
  public Html show(){
    if(Html == null){
      this.html = new Html(url);
      System.out.println("Proxy " + url);
    }
    System.out.println("PRoxy cache " + url);
    return html;
  }
}

public class AopBrowser implemnet IBrowser{
  private String url;
  private Html html;
  private Runnable before;
  private Runnable after;

  public AopBrowser(String url, Runnable before, Runnable after){
    this.url = url;
    this.before = before;
    this.after = after;
  }

  @Override
  public Html show(){
    before.run();

    if(Html == null){
      this.html = new Html(url);
      System.out.println("AopProxy " + url);
      Thread.sleep(1500);
    }
    after.run();

    System.out.println("AopPRoxy cache " + url);
    return html;
  }

}
public class Main{
  public static void main(String[] args){
    //Browser browser = new Browser("naver.com")
    //browser.show();

    //IBrowser browser = new BrowserProxy("naver.com")
    //browser.show();
    //browser.show(); //use cache

    Browser browser = new AopBrowser("naver.com",
     () -> {
      start.set(System.currentTimeMillis());
    },
     () -> {
       long new = System.currentTimeMillis();
       ent.set(now - start.get());
     });
     browser.show(); //1.5s
     browser.show(); //0s
  }
}
```
