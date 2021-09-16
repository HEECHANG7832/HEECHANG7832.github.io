### 내부 클래스

인스턴스 클래스



스태틱 클래스



local 클래스



익명 클래스

클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스(일회용)

단 하나의 객체만을 생성할 수 있다



특징들

static클래스만 static멤버를 정의할 수 있다

내부 클래스도 외부 클래스의 멤버이며 동일한 접근성을 가진다

외부 클래스의 지역변수는 final이 붙은 변수만 접근가능하다



### 컬렉션 프레임웤

다수의 데이터를 쉽게 처리할 수 있는 방법을 제공

컬렉션 - 다수의 데이터, 데이터 그룹

프레임웤 - 표준화, 정형화된 체계적인 프로그램이 방식

컬렉션 클래스 - 다수의 데이터를 저장할 수 있는 클래스



List

순서가 있는 데이터의 집합

Set

순서가 없고 중복을 허용하지 않음

Map

키와 값의 쌍으로 이루어진 데이터의 집합

키는 중복 허용하지 않음



**Collection Framework의 동기화**

멀티 쓰레드 환경에서 Collection Framework의 동기화가 필요

Vector 같은 구버전 클래스는 자체적인 동기화

ArrayList는 별도의 동기화 처리 필요

Collections클래스는 다음과 같은 동기화 처리 메서드를 제공한다



**Vector, ArrayList**

ArrayList는 기존의 Vector를 개선한 것 원리와 기능 동일

List를 구현



**Vector 메서드**





**ArrayList단점**

크기를 변경할 수 없다. 크기 변경시 새로운 배열 생성후 데이터 복사

비순차적인 데이터의 추가시 많은 데이터를 옮겨야함

순차적인 데이터 추가와 삭제는 빠르다



Deep copy

같은 내용의 새로운 객체를 생성

원본의 변화에 복사본이 영향을 받지 않는다

Shallow copy

참조변수만 복사 원본의 변화에 복사본이 영향을 받음



**LinkedList** 주요 메서드



**스택과 큐**



**Enumeration(구버전), Iterator, ListIterator(양방향으로 접근성 향상)**

컬랙션 클래스에 저장된 데이터를 접근하는데 사용되는 인터페이스이다.

메서드들



**HashSet**

중복허용x 순서유지x

hashCode() 사용



**TreeSet**

이진검색트리의 구조

검색과 정렬에 유리, 데이터 추가 삭제시간이 더 걸린다

주요 메서드



**Comparator(기본 정렬기준 외 다른 기준), Comparable(기본 정렬기준)**

객체를 정렬하는데 필요한 메서드를 정의한 인터페이스이다

이들에 정의된 compare(), compareTo()를 구현해서 정렬이 필요한 경우



**Hashtable, HashMap(신버전, 동기화 됨)**

해싱 기법을 사용해서 많은 데이터를 찾는데 성능이 뛰어나다

주요 메서드



해싱에 사용되는 자료구조는 배열과 링크드리스트가 조합된 형태



**TreeMap**

주요 메서드



**Properties**

내부적으로 Hashtable을 사용하며 key와 value를 저장

어플리케이션의 환경설정에 관련된 속성을 저장하는데 사용되며 파일로부터 편리하게 값을 읽고 쓸 수 있는 메서드를 제공



요약

ArrayList

LinkedList

HashMap

TreeMap

Stack

Queue

Properties

HashSet

TreeSet

LinkedHashMap

LinkedHashSet





### 쓰레드

```java
class MyThread extends Thread{
    public void run(){
        
    }
}

class MyThread implements Runnable{
    public void run(){
        
    }
}
Thread1 t1 = new Thread1();
t1.start();

//우선순위 부여
void setPriority(int newPriority);
int getPriority();
```



**쓰레드 그룹**

서로 관련된 쓰레드를 그룹으로 묶어서 다루기 위한 것

모든 쓰레드는 반드시 하나의 쓰레드 그룹에 포함

기본은 main 쓰레드

자신을 생성한 쓰레드의 그룹과 우선순위를 상속받는다



**데몬 쓰레드**

일반 쓰레드가 모두 종료되면 자동 종료, 가비지 컬렉터, 자동저장, 화면자동갱신 등



**쓰레드의 실행제어**

메서드들~



**쓰레드의 상태**

NEW

RUNNABLE

BLOCKED

WAITING

TIMED_WAITING

TERMINATED



**쓰레드의 동기화**

//객체의 lock

synchronized(객체의 참조변수){

}

//메서드에  lock

public synchronized void calcSum(){

}

wait() 객체의  lock을 풀고 해당 객체의 쓰레드를 waiting pool에 넣는다

notify()  waiting pool에 대기중인 쓰레드 중 하나를 깨운다

notifyAll() waiting pool



### 입출력

**스트림**

데이터를 운반하는데 사용되는 연결통로

하나의 스트림으로 입출력을 동시에 수행할 수 없다(단방향)

입출력을 동시에 수행하려면 2개의 스트림이 필요



**바이트 기반 스트림**

데이터를 바이트 단위로 주고받는다

InputStream, OutpurStream

메서드들~



Byte[]

ByteArrayInputStream

ByteArrayOutputStream



File

FileInputStream

FileOutputStream



PrintStream

System.out과 System.err가 PrintStream이다



**문자 기반 스트림**

입출력 단위가 문자(char, 2byte)인 스트림

Reader, Writer

메서드들~



File

FileReader, FileWriter

프로세스간 통신

PipedReader, PipedWriter



StringReader, StringWriter



문자기반 보조스트림

```java
FileReader fr = new FileReader("1.txt");
BufferedReader br = new BufferedREader(fr);
String line = br.readLine();
```





**표준 입출력**



**직렬화**



**네트워킹**



**소켓프로그래밍**

