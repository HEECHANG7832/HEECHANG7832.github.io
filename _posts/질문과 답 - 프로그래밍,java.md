# 프로그래밍



### 객체지향이 무엇인가요? 절차지향과의 차이점은 뭐죠?

- OOP (객체지향 프로그래밍) **(중요)**
  객체지향 프로그래밍은 컴퓨터 프로그래밍 패러다임(견해, 사고법)의 하나로, **프로그래밍에서 필요한 데이터를 추상화 시켜서 상태(속성, 어트리뷰트)와 행위(메서드)를 가진 객체** 로 만들고, 그 객체간의 상호작용을 통해 로직을 구성하는 방법.
  
  - 장점
    - 다른 클래스를 가져와 사용할 수 있고, 상속받을 수 있어 코드의 재사용성 증가
    - 절차지향보다 유지보수가 간단
    - 클래스 단위로 모듈화가 가능하여, 대형 프로젝트에 적합
  - 단점
    - 처리속도가 상대적으로 느리다.
    - 객체가 많으면 용량이 커진다.
    - 설계시 많은 노력과 시간이 필요하다.
    
    

### 객체지향 SOLID 원칙에 대해서 설명해 주세요.

- OOP의 5가지 법칙 (SOLID)
  - Single Responsibility Principle, 단일 책임 법칙
    각 클래스는 목적을 하나씩만 가지고 그에 대한 책임을 져야 한다.
  - Open Close Principle, 개방 폐쇄 법칙
    각 클래스는 클래스에 대한 수정을 폐쇄하고, 확장에 대해 개방해야 한다.
    즉 클래스를 수정해야 한다면 그 클래스를 상속, 즉 확장하여 수정한다.
  - Liskov Substitusion Principle, 리스코프 치환 법칙
    자식 클래스를 사용 중일때, 거기에 부모 클래스로 치환하여도 문제가 없어야 한다.
  - Interface Segreation Principle, 인터페이스 분리 법칙
    각 행위에 대한 인터페이스는 서로 분리되어야 한다.
    핸드폰을 예로 들면, 전화를 하는데 핸드폰 카메라가 방해가 되면 안된다는 말.
  - Dependency Inversion Principle, 의존성 역전 법칙
    상위 클래스가 하위 클래스에 의존하면 안된다는 법칙. 즉 기본적인 공통되는 속성을 하위 클래스에 의존하면 안된다.

### 객체지향 4가지 특징에 대해서 설명해 주세요.

- **추상화**
  - 객체지향 관점에서 클래스를 정의하는 것, 불필요한 정보 외 중요한 정보만 표현함으로써 공통의 속성과 기능을 묶어 이름을 붙이는 것.
- **캡슐화**
  - 코드를 수정없이 재활용 하는 것을 목적으로 함. 클래스라는 캡슐에 기능과 특성을 담아 묶는다. 목적을 기준으로 묶는다. 은닉화와의 차이 - 은닉화는 캡슐화의 일부라고 볼 수 있으며, 목적으로 묶인 캡슐 안을 사용자는 볼 수 없다는 것이 은닉화.
- **상속**
  - 클래스로부터 속성과 메서드를 물려받는 것을 말함. 다른 클래스를 가져와서 수정할 일이 있다면, 그 클래스를 직접 수정하는 대신 상속을 받아 변경하고자 하는 부분만 변경
- **다형성**
  - 하나의 변수명이나 함수명이 상황에 따라 다르게 해석될 수 있음. 대표적인 다형성이 오버라이딩과 오버로딩



추가

- 클래스란?

  - 현실 세계의 객체를 추상화시켜, 속성과 메서드로 정의한 것 (논리적 개념)

- 인스턴스란?

  - 클래스에서 정의한 것을 토대로 만든 실제 메모리상에 할당된 것, 실제 데이터

  

### 데이터 타입과 변수의 차이는 무엇인가요?

**변수**

- 값을 저장할 수 있는 메모리상의 공간, 값의 위치를 기억하는 저장소

**데이터 타입**

- 메모리에 값을 저장하기 위해서는 먼저 메모리 공간을 확보해야 할 메모리의 크기([byte](https://ko.wikipedia.org/wiki/바이트))를 알아야한다. 이는 값의 종류에 따라 확보해야 할 메모리의 크기가 다르기 때문이다. 이때 값의 종류, 즉 데이터의 종류를 데이터 타입(Data Type)이라 한다.

https://poiemaweb.com/js-data-type-variable



### 함수형 프로그래밍에 대해서 설명해 주세요.

함수형 프로그래밍의 컨셉

- 변경 가능한 상태를 불변상태로 만들어 sideEffect을 제거
- 모든 것은 객체
- 코드는 간결하게, 로직에 집중
- 동시성 작업을 보다 쉽게

#### 함수형 프로그래밍 조건



일반적으로 함수형 프로그래밍에서는 다음 세가지 조건을 만족시켜야 합니다.

#### `순수 함수(pure function)`



순수 함수란 같은 입력에 대해 항상 같은 출력을 반환하는 함수로 다음과 같은 조건을 만족하는 함수를 말합니다. 멀티쓰레드에서도 안전하고 병렬처리 및 계산도 가능합니다.

- 동일한 입력에 대해 항상 같은 값을 반환.
- 부작용(다른 요인에 따른 결과변경)이 없는 결과를 생성. -> 함수에서 인자를 변경하거나 프로그램의 상태를 변경하지 않음.

#### `고차함수(High Order Function)`



1급 함수의 서브셋으로 다음 조건을 만족하는 함수를 말합니다.

- 함수의 인자로 함수를 전달할 수 있다.
- 함수의 리턴값으로 함수를 사용할 수 있다.

#### `익명 함수(Anonymous function)`



이름이 없는 함수를 말하며 람다식으로 표현되는 함수 구현을 말합니다.



- 함수형 프로그래밍은 선언형 프로그래밍으로, 어떻게(How)가 아닌 무엇(What)을 정의한다. C, Java등의 언어는 명령형 프로그래밍이며, 알고리즘을 기술하고 목적은 기술하지 않는다. 선언형은 반대로 알고리즘은 기술하지 않고 목적 위주로 기술하며, 데이터의 입력이 주어지고 데이터의 흐름을 추상적을 정의하는 방식.



**각 배열의 원소에 자기자신을 곱하는 코드**

- 배열도 만들지 않고 for문도 사용하지 않는다

```java
Arrays.asList(1,2,3).stream()
	.map(i -> i*i)
	.forEach(System.out::println);

```



함수형 인터페이스를 활용한 구현

함수형 인터페이스 - 단일 추상 메서드를 가지는 인터페이스

```java
interface MyInterface {
	void printMsg();
}

//일반적인 구현
class MyClass implements MyInterface {
	void printMsg(String msg) {
		System.out.println(msg);
	}
}

class MyApp {
	MyApp() {
		MyClass mc = new MyClass();
		print(mc);
	}
	...
	void print(MyInterface arg) {
		arg.printMsg("Hello");
	}
}

//함수형 인터페이스를 이용한 구현
class MyApp {
	MyApp() {
		print(msg -> System.out.println(msg));
	}
	...
	void print(MyInterface arg) {
		arg.printMsg("Hello");
	}
}
```



java.util.function 패키지

```java
//( parameters ) -> expression body       // 인자가 여러개 이고 하나의 문장으로 구성
//( parameters ) -> { expression body }   // 인자가 여러개 이고 여러 문장으로 구성
//() -> { expression body }               // 인자가 없고 여러 문장으로 구성
//() -> expression body                   // 인자가 없고 하나의 문장으로 구성

Function<String, Integer> f = str -> Integer.parseInt(str);
Integer result = f.apply("10");
```



https://dinfree.com/lecture/language/112_java_9.html

https://medium.com/@lazysoul/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80-d881230f2a5e



### 1급 객체에 대해서 설명해 주세요.

- 변수나 데이타에 할당 할 수 있어야 한다.
- 객체의 인자로 넘길 수 있어야 한다.
- 객체의 리턴값으로 리턴 할수 있어야 한다

자바스크립트의 경우 함수는 객체 이므로 함수가 1급 객체가 되는 것이며 자바의 경우 함수형 인터페이스(추상메서드가 하나인 인터페이스)를 통해 구현이 가능합니다

https://medium.com/@lazysoul/functional-programming-%EC%97%90%EC%84%9C-1%EA%B8%89-%EA%B0%9D%EC%B2%B4%EB%9E%80-ba1aeb048059





### 컴파일러와 인터프리터의 차이는 무엇인가요?

**컴파일러**

프로그래밍 언어를 Runtime 이전에 기계어로 해석하는 작업 방식이다.
이때 원래의 소스를 원시 코드, 바뀐 코드를 목적 코드(Object Code) 라 한다.

런타임 이전에 Assembly 언어로 변환하기 때문에 구동 시간이 오래걸리지만, 구동된 이후는 하나의 패키지로 매우 빠르게 작동하게 된다.
구동시에 코드와 함께 시스템으로부터 메모리를 할당받으며 할당받은 메모리를 사용하게 된다.

런타임 이전에 이미 해석을 마치고 대게 컴파일 결과물이 바로 기계어로 전환되기 때문에 OS 및 빌드 환경에 종속적이다.그러므로 OS 환경에 맞게 호환되는 라이브러리와 빌드환경을 구분해서 구축해줘야 한다.

**인터프리터**

런타임 이전에 기계어로 프로그래밍 언어를 변환하는 컴파일 방식과 다르게, 런타임 이후에 Row 단위로 해석(Interpret) 하며 프로그램을 구동시키는 방식이다.

프로그래밍 언어를 기계어로 바로 바꾸지않고 중간 단계를 거친 뒤, 런타임에 즉시 해석하기 때문에 바로 컴팩트한 패키지 형태로 Binary 파일을 뽑아낼 수 있는 Compile 방식에 비해 낮은 퍼포먼스를 보이게 된다.

런타임에 직접 코드를 구동시키는 특징이 있기 때문에 실제 실행시간은 느리며, 대신 런타임에 실시간 Debugging 및 코드 수정이 가능하다.

또한 메모리를 별도로 할당받아 수행되지 않으며, 필요할 때 할당하여 사용한다. 이와 관련되어 코드의 흐름 자체도 실제 필요할 때, 실제 수행되어야하는 시점에 수행되기 때문에 덕타이핑(Duck Typing) 이 가능한 측면이 있으나, 반대로 정적 분석이 되지않는 Trade off 를 갖고 있다.

https://jins-dev.tistory.com/entry/Compiler-%EC%99%80-Interpreter-%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%B0%A8%EC%9D%B4%EC%A0%90



### 오버로딩과 오버라이딩의 차이는 무엇인가요?



**오버로딩**

- **자바의 한 클래스 내에 이미 사용하려는 이름과 같은 이름을 가진 메소****드가 있더라도 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메소드를 정의할 수 있다.**

- **메소드의 이름이 같고, 매개변수의 개수나 타입이 달라야 한다.** 주의할 점은 **'리턴 값만'** **다른 것은 오버로딩을 할 수 없다는 것이다.**



**오버라이딩**

- **부모 클래스로부터 상속받은 메소드를 자식 클래스에서 재정의**

- 오버라이딩은 부모 클래스의 메소드를 재정의하는 것이므로, 자식 클래스에서는 **오버라이딩하고자 하는 메소드의 이름, 매개변수, 리턴 값이 모두 같아야 한다.**

### RxJava

**리액티브 프로그래밍**

리액티브 프로그래밍은 **데이터 흐름과 전달에 관한 프로그래밍 패러다임**이다. 기존의 명령형(imperative) 프로그래밍은 주로 컴퓨터 하드웨어를 대상으로 프로그래머가 작성한 코드가 정해진 절차에 따라 순서대로 실행된다. 그러나 리액티브 프로그래밍은 데이터 흐름을 먼저 정의하고 데이터가 변경되었을때 연관되는 함수나 수식이 업데이트되는 방식이다.

리액티브 프로그래밍에서는 데이터의 변화가 발생했을때 변경이 발생한 곳에서 새로운 데이터를 보내(push 방식)준다. **기존 자바 프로그래밍이 pull 방식이라면 리액티브 프로그래밍은 push 방식이다.**

콜백이나 옵저버 패턴을 넘어서 RxJava 기반의 리액티브 프로그래밍이 되려면 함수형 프로그래밍이 필요하다. **콜백이나 옵서버 패턴은 옵서버가 1개이거나 단일 스레드 환경에서는 문제가 없지만 멀티 스레드 환경에서는 사용할때 많은 주의가 필요하다. 대표적인 예가 데드락과 동기화 문제이다.**

함수형 프로그램이은 부수 효과가 없는 순수 함수(pure function)를 지향한다. 따라서 멀티 스레드 환경에서도 안전하다. 자바 언어로 리액티브 프로그래밍을 하기 위해서는 함수형 프로그래밍의 지원이 필요하다.

애플리케이션에서 리액티브 프로그래밍을 하려면 누군가 리액티브 프로그래밍을 할 수 있는 기반 시설을 제공해주어야 한다. 즉, 데이터 소스를 정의할 수 있고 그것의 변경사항을 받아서 프로그램에 알려줄(push) 존재가 필요하다. JVM 위의 자바 언어로 구현해놓은 라이브러리가 RxJava이다.

https://12bme.tistory.com/570

### JAVA 8 메서드 레퍼런스

메서드 레퍼런스는 특정 메서드만을 호출하는 람다의 축약 표현이라고 생각할 수 있다.



(Car c) -> c.getPrice()                                       =========>              Car :: getPrice
() -> Thread.currentThread().dumpStack()       =========>              Thread.currentThread()::dumpStack
(str, i) -> str.substring(i)                                    =========>              String::substring
(String s) -> System.out.println(s)                    =========>              System.out::println

메서드 레퍼런스는 하나의 메서드를 참조하는 람다를 편리하게 표현할 수 있는 문법이다.



## JAVA

### 기본

- Java 접근 제어자에 대해서 각각 설명해 주세요.
  - https://wikidocs.net/232
- JVM의 구조에 대해서 설명해 주세요.
  - https://jeong-pro.tistory.com/148
- Garbage Collector 에 대해서 설명해 주세요. 어떻게 동작하나요?
- GC의 종류에 대해서 말해보세요.
  - https://velog.io/@hygoogi/%EC%9E%90%EB%B0%94-GC%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C
- Java 버전 별 특성에 대해서 아는대로 말해주세요.
  - https://kudl.tistory.com/entry/JAVA-%EB%B2%84%EC%A0%84%EB%B3%84-%ED%8A%B9%EC%A7%95
- Java는 Call By Value일까요, Call By Reference 일까요?
  - https://re-build.tistory.com/3
  - https://hyoje420.tistory.com/6
- 리플렉션(Reflection)이란 무엇인가요?
  - https://dublin-java.tistory.com/53
  - https://gyrfalcon.tistory.com/entry/Java-Reflection
- Stream API란 무엇인가요?
  - https://velog.io/@litien/Stream-API
- Lambda란 무엇인가요?
  - https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95
- 함수형 인터페이스란 무엇인가요?
  - https://codechacha.com/ko/java8-functional-interface/
- JVM 기동시 주로 사용되는 옵션들을 아는대로 말해보세요.
- foreach를 사용할 수 있는 자료구조는 어떤 인터페이스를 상속받고 있나요?
- iterator와 iterable 차이는 무엇인가요?
  - https://woowacourse.github.io/javable/post/2020-08-31-java-loop/
- synchronized 키워드에 대해 설명해 주세요.
  - https://tourspace.tistory.com/54
- volatile 키워드에 대해 설명해 주세요.\
  - https://nesoy.github.io/articles/2018-06/Java-volatile
- final 키워드에 대해서 설명해주세요. 각각의 쓰임에 따라 어떻게 동작하나요?
  - https://blog.lulab.net/programming-java/java-final-when-should-i-use-it/

### 클래스와 객체

- Wrapper Class란 무엇인가요?
  - https://coding-factory.tistory.com/547
- 클래스, 객체, 인스턴스 차이에 대해서 설명해 주세요.
  - https://gmlwjd9405.github.io/2018/09/17/class-object-instance.html
- 직렬화(Serialization)과 역직렬화(Deserialization)에 대해서 설명해 주세요.
  - https://sas-study.tistory.com/345
- Java Generic에 대해서 설명해 주세요.
  - https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3
- equals와 ==의 차이는 무엇인가요?
  - https://ojava.tistory.com/15
- hashCode란 무엇인가요?
  - https://brunch.co.kr/@mystoryg/133
- 문자열을 리터럴(`string = "abcd"`)로 할당하는 것과 객체(`string = new String("abcd")`)로 할당하는 방식의 차이가 무엇인가요?
  - https://madplay.github.io/post/java-string-literal-vs-string-object
- 순수 추상 클래스와 인터페이스의 차이는 무엇인가요?
  - https://haenny.tistory.com/162
- 본인 관점에서, 인터페이스는 주로 어떨 때 사용하나요?
  - https://gofnrk.tistory.com/22

### 자료형, 자료구조

- Java의 Collection에 대해서 설명해 주세요.
- Array와 ArrayList의 차이점은 무엇인가요?
- char type과 string type으로 나뉘어져 있는 이유는 무엇인가요?
  - https://devlog-wjdrbs96.tistory.com/135?category=882228



### 부동소수점

https://steemit.com/kr/@modolee/floating-point



기본적인 SELECT 
JOIN 
Isolation Level

call by (value|reference|sharing)
리틀 엔디안 / 빅 엔디안



### 자바 키트별 차이

https://medium.com/@logishudson0218/jdk-sdk-ndk-3b095101c040



사용자 수준 스레드, 커널수준 스레드



### 1급 객체

https://velog.io/@reveloper-1311/%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4First-Class-Object%EB%9E%80



### RestTemplate

https://sjh836.tistory.com/141



Nginx(Apache)
관계형데이터베이스,
분산데이터베이스

### 프로세스 생성 과정에 대해서 설명해보세요

http://yimoyimo.tk/OS-Process-Creation-and-Termination/

https://hack-gogumang.tistory.com/256



### 인터프리터, 컴파일 언어 차이

인터프리터 언어는 원시코드(프로그래머가 작성한 소스코드)를 기계어로 변환하는 과정없이 한줄 한줄 해석하여 바로 명령어를 실행하는 언어를 말합니다. R, Python, Ruby와 같은 언어들이 대표적인 인터프리터 언어입니다.

인터프리터가 직접 한 줄씩 읽고 따로 기계어로 변환하지 않기 때문에 빌드 시간이 없습니다. Runtime 상황에서는 한 줄씩 실시간으로 읽어서 실행하기 때문에 컴파일 언어에 비해 속도가 느립니다.

실행속도는 느리지만 코드 변경시 빌드 과정없이 바로 실행이 가능하다는 장점이 있습니다. 루비를 사용해보면 소스코드를 고치고 서버를 다시 시작하지 않아도 변경사항이 반영된 상태로 테스트를 진행할 수 있습니다.

컴파일 언어는 원시코드(프로그래머가 작성한 소스코드)를 모두 기계어로 변환한 후에 기계(JVM 같은 가상 머신)에 넣고 기계어 코드를 실행합니다. 소스코드를 기계어로 번역하는 빌드 과정에서는 인터프리터 언어에 비해 시간이 소요됩니다. 하지만 런타임 상황에서는 이미 기계어로 모든 소스코드가 변환되어 있기 때문에 빠르게 실행할 수 있습니다. 대표적인 언어로  C, C++이 있습니다.



DB 다시 처음부터
DB문제 고민
filter와 interpreter 차이
JVM
GC
instanceOf
foreach내부동작
switch과if차이
객체지향특징, 5원칙
오버로딩/라이딩 차이 및 제한조건
this / super
interface / abstract 차이
객체생성순서 / Object클래스
다이나믹 메소드 디스패치
String 왜 final인지 왜 불변인지
String +연산시 객체 몇개 만들어지는지
hashMap 충돌방지 방법
각종 메서드 시간복잡도
각종 JAVA 키워드
예외 종류 ,커스텀예외
제네릭
디자인패턴
어노테이션
call by value
람다
enum
쓰레드 / 동기화