### JAVA 기본

```JAVA
String str = new String("abc");

//true는 예약어
//숫자로 시작x, $,_만 사용가능
//지역변수는 반드시 사용하기전 초기화

//anytype + 문자열 = 문자열

//연산자
//! 논리부정
//~ 비트반전

//byte short 연산은 int로
//피연산자중 표현 범위가 큰쪽으로 형변환

float pi = 3.141592f;
Math.round(pi); //소수 첫째자리에서 반올림
    
Math.randoom(); //0과 1.0사이의 double 값 반환

//배열 선언
int[] score;
String[] name;
int score[];
String name[];

int[] score = new int[5];
score.length;

//다차원 배열
int[][] score;
int score[][];
int[] score[]; //??
int[][] score = new int[5][3];

//가변배열
int[][] score = new int[5][];
score[0] = new int[3];
score[1] = new int[4];

//배열 복사
System.arraycopy(arr1, 0, arr2, 0, arr1.length);

//매개변수
public static void main(String[] args){
	args.length;
	args[i];
}
```



### JAVA 특징들

클래스 - 객체를 정의한것, 생성하는데 사용

객체 - 실제로 존재하는 것, 속성과 기능에 따라 용도가 다름

인스턴스



객체의 구성요소

속성(변수), 기능(메서드)



하나의 인스턴스를 여러개의 참조변수가 가르키는 경우는 되지만

여러개의 인스턴스를 하나의 참조변수가 가르킬 수는 없다

```java
//Scope
class Time{
    int i; //인스턴스 변수, 각 인스턴스의 개별적인 저장공간, 인스턴스 생성시 생성, 참조변수가 없으면 자동제거
    static int j; //클래스 변수(static), 클래스가 로딩될때 생성, 프로그램 종료시 소멸, 같은 클래스의 모든 인스턴스가 공유, 인스턴스 생성 없이 사용
    
    void method()
    {	
        int i = 0; //지역변수, 메서드 종료와 함께 소멸
    }
}
```



### JVM 메모리 구조

메서드 영역 - 클래스 정보와 클래스 변수가 저장

호출스택 - 메서드의 작업공간

힙 - 인스턴스가 생성되는 공간, new 연산자로 생성되는 모든 객체의 공간



인스턴스 메서드

클래스 메서드 - 객체생성없이 클래스이름.메서드이름() 으로 호출, 인스턴스변수와 관련없는 작업, 인스턴스 변수 사용불가



같은 클래스 내부에서 메서드를 바로 호출 가능하지만

클래스 메서드 내부에서는 인스턴스 메서드(변수)를 호출 할 수 없다



### 메서드 오버로딩

하나의 클래스에 같은 이름의 메서드를 여러개 정의하는것

이름 같고, 매개변수 **개수 또는 타입**이 다르다. 리턴타입은 상관없다



### 생성자

인스턴스 생성시 호출되는 초기화 메서드

모든 클래스에는 반드시 하나 이상의 생성자가 있다

생성자의 이름은 클래스의 이름과 같다. 리턴은 없음

**this()** 생성자에서 다른 생성자 호출(첫 문장에서만 가능)

**this** 인스턴스 자기자신을 가리키는 참조변수



```java
class Car{
    //생략
    Car(Car c){
        //인스턴스 초기화
    }
}
Class CarTest{
    
    public static void main(String[] args){
        Car c1 = new Car();
        Car c2 = new Car(c1); //생성자를 통한 복사
    }
}
```



### 상속

기존의 클래스를 재사용해서 새로운 클래스를 작성

자손은 조상의 모든 멤버를 상속받는다.(생성자, 초기화블럭 제외)

공통부분은 조상에서 관리 개별부분은 자손

포함관계 - 한 클래스의 멤버변수로 다른 클래스를 선언하는 것

JAVA에서는 단일상속만 허용

모든 클래스는 자동으로 Object클래스를 상속받는다 toString(), equals(Object obj), hashCode()...



### 오버라이딩

조상클래스로부터 상속받은 메서드의 내용을 상속받는 클래스에 맞게 변경하는 것

선언부가 같아야 한다

접근제어자는 같은 범위

조상클래스보다 많은 예외 선언 불가

**super** 조상의 멤버와 자신의 멤버를 구분할때 사용

**super()** 조상의 생성자 자손생성자의 처음에 호출되어야한다



### Package, Import

서로 관련된 클래스, 인터페이스의 묶음

물리적으로 폴더, 서브패키지를 가질수 있다



package com.java.book; //소스파일 첫 문장에 단 한번 선언

모든 클래스는 패키지에 속하며, 패키지가 선언되지 않으면 자동으로 이름없는 패키지에 속하게 된다



클래스 패스

classpath는 클래스파일을 찾는 경로



import

사용할 클래스가 속한 패키지를 지정하는데 사용

import사용후 패키지명 생략 가능

java.lang 패키지의 클래스는  import하지 않고 사용 ,String, Object, System, thread 등

일반적으로 package문, import문, 클래스 선언 순

import문은 컴파일 시에 처리됨

이름이 같은 클래스가 속한 패키지 import시에는 패키지를 명시해야함



### 제어자

static

클래스 변수로 만들거나 인스턴스 생성없이 호출이 가능한 static 메서드가 된다

final

변경, 확장이 불가능한 클래스, 메서드, 상수

인스턴스 변수로 선언한 경우 생성자에서 한번 초기화 가능

abstract

클래스 내에 추상메서드가 있음을 의미

선언부만 작성하고 구현은 없는 추상메서드



**접근제어자**

외부로부터 데이터를 보호, 내부적으로만 사용되는 부분을 감추기 위해서

private - 같은 클래스

default - 같은 클래스

protected - 같은 패키지 내에서, 그리고 다른 패키지의 자손 클래스

public - 접근 제한 없음



생성자의 접근제어자는 클래스의 접근제어자와 일치

생성자에 접근제어자를 사용해 인스턴스의 생성을 제한할 수 있다 Singleton pattern

메서드에 static과 abstract는 함께 사용 x

클래스에 final과 abstract는 함께 사용 x

abstract메서드의 접근제어자가 private일 수 없다

private인 메서드는 오버라이딩 될 수 없으니 final을 쓸 필요가 없다



### 다형성

여러가지 형태를 가질 수 있는 능력

하나의 참조변수로 여러 타입의 객체를 참조할 수 있는 것

조상타입의 참조변수로 자손타입의 객체를 다룰 수 있는 것이 다형성

서로 상속관계에 있는 타입간 형변환이 가능

instanceof 연산자

fe instanceof Car 



멤버변수가 중복정의된 경우 참조변수의 타입에 따라 연결되는 멤버가 달라진다

메서드가 중복정의된 경우 항상 실제 인스턴스의 타입에 따라 메서드가 호출됨

조상타입 배열에 자손들의 객체를 담을 수 있다



**Vector**

Vector vec = new Vector();

vec.add(Object o);

vec.remove(Object o);

vec.isEmpty(Object o);

vec.get(int index);

vec.size()



### 추상클래스

미완성 메서드를 포함하고 있는 클래스

추상클래스를 상속받으면 반드시 구현부를 완성해야 한다

여러 클래스에서 공통적으로 사용될 수 있는 부분을 뽑아서 추상클래스를 만든다



### 인터페이스

일종의 추상클래스지만 좀더 추상화 정도가 높다

추상메서드와 상수만을 멤버로 가진다

인스턴스 생성x 클래스 작성에 도움을 준다

표준을 제시하는데 사용됨

다중상속이 허용됨, 최고조상이 없다

상속과 구현이 동시에 가능

인터페이스 타입의 변수로 인터페이스를 구현한 클래스의 인스턴스를 참조할 수 있다

인터페이스를 메서드의 매개변수 타입으로 지정가능

인터페이스를 리턴타입으로 지정할 수 있다

장점

1. 개발시간 단축 - 메서드를 호출하는 쪽에서 메서드의 내용에 관계없이 선언부만 알면 되기 때문
2. 표준화 가능
3. 서로 관계없는 클래스들에게 관계를 맺어 줄 수 있다
4. 독립적인 프로그래밍 가능 - 클래스의 선언과 구현을 분리시킬 수 있기 때문에, 클래스와 클래스간의 직접적인 관계를 인터페이스를 이용해서 간접적인 관계로 변경하면, 한 클래스의 변경이 관련된 다른 클래스에 영향을 미치지 않는 독립적인 프로그래밍이 가능하다



두 객체 간의 연결을 돕는 중간 역활

선언과 구현을 분리

클래스를 사용하는 쪽과 클래스를 제공하는 쪽이 있는데 메서드를 사용하는 쪽에서는 사용하는 메서드의 선언부만 알면 된다

```java
//직접적인 관계
class A{
    public void methodA(B b){
        b.methodB();
    }
}

class B{
    public void methodB(){
        System.out.println("method");
    }
}
class InterfaceTest{
    pulbic static void main(String args[]){
        A a = new A();
        a.methodA(new B());
    }
}

//간접적인 관계
class A{
    public void methodA(I i){
        i.methodB();
    }
}

interface I{
    void methodB();
}
class B implements I{
    public void methodB(){
        System.out.println("method");
    }
}

class C implements I{
    public void methodB(){
        System.out.println("method in C")
    }
}
```



### 예외처리

컴파일 에러

런타임 에러

에러 - 수습할 수 없는 오류

예외 - 코드에 의해 수습가능한 미약한 오류

예외처리란

프로그램 실행 시 발생할 수 있는 예외의 대비한 코드를 작성하는 것

비정상 종료를 막고 정상적인 실행상태를 유지하는 것

### 예외 처리

try-catch

```
try{

}catch(Exception e1){

}

//예외 고의로 발생시키기
Exception e = new Exception("ee")
throw e;

```

RuntimeException 클래스  
프로그래머의 실수에 의해 발생하는 예외  
외예외 처리 필수

Exception 클래스  
사용자의 실수와 외적인 요인에 의한 예외

예외의 최고 조상인 Exception 클래스는 마지막 catch 블럭이어야 한다

ae.printStackTrace() 예외 발생 당시의 호출 스택에 있던 메서드의 정보와 메시지 같이 출력  
ae.getMessage() 발생한 예외 클래스의 인스턴스에 저장된 메세지 출력

finally 블럭  
무조건 실행되는 블럭 try-catch블럭에서 return을 만나도 실행된다

서메서드에서 예외 처리하기  
예예외가 발생하면 함수를 호출한쪽에 예외를 던져준다

```
void method() throws Exception1, Exception2, {
    //contents
}
```

예외는 호출하는 쪽 호출 받는쪽 어디서든 처리할 수 있지만  
호호출받는 쪽에서 리이미 처리하더라도 다시 예외를 발생시켜 호출한 쪽에 알려줄 수 있다 - 예외 되던지기

사용자 의정의 예외 만들기

```
class MyException extends Exception{
    MyException(String msg){
        super(msg)
    }
}
```

### java.lang 패키지

Objecg클래스

모든 클래스의 조상  
equals() hashCode() toString()  
등은 적절히 오버라이딩 해야된다

equals(Object obj)  
객체 자신과 주어진 객체를 비교 오리지날은 잠조변수 값(객체의 주소)를 비교한다

hashCode()  
객체의 해시코드를 반환하는 해시함수  
오오리지날객체의 주소값으로 해시코드를 생성함으로 두 인스턴스의 해시코드 반환이 같을 수 없다  
equals를 오버라이딩하면 이 함수도 오버라이딩 해야한다

toString()  
객체의 정보를 문자열로 제공할 목적으로 정의된 메서드

clone()  
객체 자신을 복애제해서 새로운 객체를 생성하는 메서드  
Cloneable인터페이스를 구하현한한 클래스의 인스턴스만 복제할 수 있다

### String 클래스

문자형 배열과 그에 관련된 메스드들 정의  
String 인스턴스의 내용은 바꿀 수 없다

String s = "";  
char c = ' ';

### String 생성자와 메서드

드복사

사값변환방법  
String str1 = 100 + "";  
String str2 = String.valueOf(100);

int i = Integer.parseInt("100");  
int i2 = Integer.valueOf("100");  
char c = "A".charAt(0);

### StringBuffer 클래스

String과 동일하게 배열(char\[\])을 가지고 있다  
String클래스와 다르게 내용을 변경할 수 있다  
equrls()를 오버라이딩 하지 않았다

다인스턴스를 생성할 때 버퍼의 크기를 충분히 지정해 주는 것이 좋다

다생성자와 메서드

드복사

### Math클래스

스복사

### Wrapper 클래스

기기본형 값을 로클래스로 정의한 것  
내부적으로 기본형 변수를 가지고 있다  
값을 비교하도록 equals()가 오버라이딩 되어 있다

### 내부 클래스

스클래스 안에클 선언된 클래스

GUI 어플의 이벤트처리에 주로 사용된다

다내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근 할 수 있다  
다코드의 복잡성을 줄일 수 있다( 캡슐화)