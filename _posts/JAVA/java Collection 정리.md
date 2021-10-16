[TOC]

### JAVA 프로그래밍 Tip



### 시간복잡도

최고차항의 차수가 BioO가 된다

주어진 함수가 아무리 나빠도 비교하는 함수와 같거나 좋다

위로 갈수록 간단하고, 아래로 갈수록 복잡해지며, \log nlog*n*은 \log_2nlog2*n*을 뜻한다.



### String

```java
String s = "java.lang.Object";
String c = s.substring(10);          
String p = s.substring(5,9);        

String animals = "dog, cat, bear";
String[] arr = animals.split(",");

String s = "abcedfg";
boolean b = s.contains("bc");

boolean b = file.endsWith("txt");

String s = "Hello";
boolean b = s.equals("Hello");

String sl = s.replace("ll","LL"));

String s = "     Hello World   ";
String sl = s.trim(); //양쪽 공백 제거

String b = String.valueOf(true);
String c = String.valueOf('a'); 
String i = String.valueOf(100); 
String l = String.valueOf(100L); 
String f = String.valueOf(10f); 
String d = String.valueOf(10.0);

java.util.Date dd = new java.util.Date();
String date = String.valueOf(dd);

//문자가 몇번째에 있는지
String str = "abcdef";
int indexOf = str.indexOf("d");

//길이
String str = "abcdef";
int length = str.length();

//서식화된 문자열
int i = 123456789;
String str = String.format("%,d", i);
```



### 정렬

```java
import java.util.Arrays;

public class Sort{
    public static void main(String[] args)  {
        int arr[] = {4,23,33,15,17,19};
        
        Arrays.sort(arr);
        Arrays.sort(arr, 0, 4); // 0,1,2,3 요소만
        Arrays.sort(arr,Collections.reverseOrder()); //내림차순
    }
}

//class로 구현
class CustomComparator implements Comparator<int[]> {
    @Override
    public int compare(int[] cost1, int[] cost2) {
        if (cost1[2] > cost2[2]) {
            return 1;
        } else if (cost1[2] == cost2[2]) {
            return 0;
        } else {
            return -1;
        }
    }
}
String[] stringArr = new String[] {"A","C","B","E","D"};
Arrays.sort(stringArr, new CustomComparator());

//익명 클래스
// 문자 길이로 sorting (오름차순)
Collections.sort(strings, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});

//익명 클래스 람다식
// 문자 길이로 sorting (오름차순)
Collections.sort(strings, (s1, s2) -> s1.length() - s2.length());
Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);
Arrays.sort(costs, (int[] c1, int[] c2) -> c1[2] - c2[2]);

//객체에 Comparable 구현
class People implements Comparable {

 	//Property
    @Override
    public int compareTo(People people) {
         // TODO Auto-generated method stub
         if (this.age < people.age) {
             return -1;
         } else if (this.age == people.age) {
             return 0;
         } else {
             return 1;
         }
     }
}

//Stream 사용
String str = "ACBED"; String[] stringArr = str.split(""); // new String[] {"A","C","F","E","D"} 배열로 변환 

String streamSortASC = 
    Stream.of(stringArr).sorted().collect(Collectors.joining()); //오름차순 String 
String streamSortDESC =
    Stream.of(stringArr).sorted(Comparator.reverseOrder()).collect(Collectors.joining()); // 내림차순

//Lambda
String streamSortASC_Lambda = 
    Stream.of(stringArr).sorted((o1,o2)->o1.compareTo(o2)).collect(Collectors.joining()); //오름차순
String streamSortDESC_Lambda = 
    Stream.of(stringArr).sorted((o1,o2)->o2.compareTo(o1)).collect(Collectors.joining()); // 내림차순

```



### JAVA Collection

https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html



**Array와 ArrayList의 다른점.**

둘 다 배열이라는 점은 동일하나, Array는 인덱스로 접근하는 반면, ArrayList는 메서드를 통해 접근한다(어짜피 Index로 호출한다는 점은 동일 하겠지만..). 또한 Array는 Object 뿐만 아니라 원시 형태(Primitive, 예를 들어 int, double 등)도 담을 수 있지만, ArrayList는 Object형(Reference, 객체)만 담을 수 있다. 따라서 정수를 ArrayList에 넣을 경우 Integer형은 가능하지만 int형은 안 된다. 덧붙여서, Integer처럼 int와 같은 원시타입을 담을 수 있는 객체를 Wrapper Class라고 한다



**List**

객체로 데이터를 다루기 때문에 적은양의 데이터만 쓸 경우 배열에 비해 차지하는 메모리가 커진다. 간단히 예로들어 primitive type인 Int는 4Byte를 차지한다. 반면에 Wraaper class인 Integer는 32bit JVM에선 객체의 헤더(8Byte), 원시 필드(4Byte), 패딩(4Byte)으로 '최소 16Byte를 차지한다. 거기에다가 이러한 객체데이터들을 다시 주소로 연결하기 때문에 16 + α 가 된다.



**ArrayList**

자바의 Vector를 개선한, **배열**로 구현된 List이다. 그 말인 즉슨, 데이터가 저장된 순서가 같다는 말이다. 사실상 배열과 같은 자료구조이기 때문에, 리스트의 연산 자체의 수행시간 속도는 배열과 같다. 모든 종류의 객체를 저장하지만 저장 및 조회시 변환해야 하므로 성능이 좋지 않다. 제네릭을 사용해 개선

```java
public class ArrayList<T>{
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ARRAY ={};
    
    private int size;
    
    Object[] array;
    
    public ArrayList(){
        this.array = EMPTY_ARRAY;
        this.size = 0;
    }
    
    public ArrayList(int capacity){
        this.array = new Object[capacity];
        this.size = 0;
    }
    
    public void resize(){   		
        if(Array.equals(array, EMPTY_ARRAY)){
            array = new Object[DEFAULT_CAPACITY];
            return;
        }
        
        if(size == array.length){
            ine new_capacity = array.length * 2;
            array = Arrays.copyOf(array, new_capacity);
            return;
        }
        
        if(size == array.length /2){
            int new_capacity = array.length / 2;
            array = Arrays.copyOf(array, Math.max(new_capacity, DEFAULT_CAPACITY));
            return;
        }
    }
    
    public void addLast(T value){
        array[size] = value;
        size++;
    }
    public void add(int index, T value){
        //에러 처리
        //마지막위치가 아닌경우
        else{
            //용량 확인
            
            for(int i = size; i > index; i--){
                array[i] = array[i - 1];
            }
            array[index] = value;
            size++;
        }
    }
    //get
    // return (T)array[index];
    //set
    // array[index] = value;
    //객체끼리 비교할때는 반드시 array[i].equals(value);
    
}
```



**LinkedList**

다음 노드의 주소를 기억하고 있는 List로, 배열에 비해 삽입과 삭제가 간단하다. 그러나 탐색의 경우 첫 번째 노드부터 탐색해 나가야 하기 때문에 느리다. 

```java
Class ListNode<T>{
    T data;
    ListNode<T> next;
    ListNode<T> prev;
    
    ListNode(T data){
        this.data = data;
        this.next = null;
        this.prev = null;
    }
}

class LinkedList<T> implement List<E>{
    private ListNode<T> head;
    private ListNode<T> tail;
    private int size;
    
    public LinkedList(){
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    
    private LinkedList<T> search(int index){
        LinkedList<T> node = head;
        for(int i = 0; i < index; i++){
            node = node.next;
        }
        return node;
    }
    
    public void addLast(T value){
        ListNode<T> newNode = new ListNode(value);
        tail.next = newNode;
        newNode.prev = tail;
        tail = newNode;
        size++;
    }
    
    public void add(int index, T value){
        
        //index 예외 처리
        //처음일 경우
        //마지막일 경우
        
        //일반적인 상황
        ListNode<T> prev_Node = search(index - 1);
        ListNode<T> newNode = new ListNode<T>(value);
        
        newNode.next = prev_Node.next;
        newNode.prev = prev_Node;
        
        prev_Node.next.prev = newNode;
        prev_Node.next = newNode;
        
        size++;
        
    }
    
    //맨 앞 지우기
    public T remove(){
        ListNode<T> headNode = head;
        E element = headNode.data;
        
        head = head.next;
        size--;
        
        return element;
    }
    //implement Cloneable
    public Object clone() throws Exception{
        LinkedList<? super T> clone = LInkedList<? super T> super.clone();
        
        clone.head = null;
        clone.tail = null;
        clone.size = 0;
        
        for(ListNode<T> x head; x != null; x = x.next){
            clone.addLast(x.data);
        }
        return clone;
    }
    
    public Object[] toArray() {
        Object[] array = new Object[size];
        int idx = 0;
        for (Node<E> x = head; x != null; x = x.next) {
            array[idx++] = (E) x.data;
        }
        return array;
    }

    @SuppressWarnings("unchecked")
    public <T> E[] toArray(E[] a) {
        if (a.length < size) {
            // Arrya.newInstance(컴포넌트 타입, 생성할 크기) 
            a = (E[]) java.lang.reflect.Array.newInstance(a.getClass().getComponentType(), size);
        }
        int i = 0;
        Object[] result = a;
        for (Node<T> x = head; x != null; x = x.next) {
            result[i++] = x.data;
        }
        return a;
    }
    public void sort() {
        /**
         *  Comparator를 넘겨주지 않는 경우 해당 객체의 Comparable에 구현된
         *  정렬 방식을 사용한다.
         *  만약 구현되어있지 않으면 cannot be cast to class java.lang.Comparable
         *  에러가 발생한다.
         *  만약 구현되어있을 경우 null로 파라미터를 넘기면
         *  Arrays.sort()가 객체의 compareTo 메소드에 정의된 방식대로 정렬한다.
         */
        sort(null);
    }

    @SuppressWarnings({ "unchecked", "rawtypes" })
    public void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);

        int i = 0;
        for (Node<E> x = head; x != null; x = x.next, i++) {
            x.data = (E) a[i];
        }
    }

}



static Comparator<T> customComp = new Comparator<T>(){
    @Override
    public int compare(T o1, T o2){
        return o2.score - o1.score;
    }
}
list.sort(customComp);
```



**Stack**

```java
public class Stack<E>{
    //기본 크기
    // 빈 배열
    
    //private void resize()
    
    public E push(E item){
        if(size == array.length){
            resize();
        }
        array[size] = item;
        size++;
        
        return item;
    }
    
    public E push(E item){
        if(size = 0){
            
        }
        
        E obj (E) array[size - 1];
        
        array[size - 1] = null;
        size--;
        resize();
        
        return obj;
    }
    
    //peek
    public E peek(){
        return (E) array[size - 1];
    }
    
}
```

E[] array 는 불가  Object[] array로 선언해야됨



**Queue**

Queue<Integer> q = new LinkedList();

```java
//circular queue
public class Queue<E>{
    //기본 사이즈
    Object[] array;
    
    private int front; //시작 인덱스 빈공간
    private int rear; //마지막 인덱스
    
    //constructor
    
    private void resize(int newCapacity){
        Object[] newArray = new Object(newCapacity);
        
        for(int i = 1, j = front + 1; i <= size; i++, j++){
            newArray[i] = array[j % arrayCapacity]
        }
        
        front = 0;
        rear = size;
    }
    
    public boolean offer(E item){
        if(rear + 1 % array.length == front){
            resize(array.length * 2);
        }
        
        rear = (rear + 1) % array.length;
        
        array[rear] = item;
        size++;
        
        return true;
    }
    
    public E poll(){
        front = (front + 1) % array.length;
        
        E item = (E) array[front];
        
        array[front] = null;
        size--;
        
        return item;
    }
}

```



```java

//입력
Scanner sc = new Scanner(System.in);
sc.nextInt();

//Array
int[] arr1 = {1, 2, 3, 4, 5};

Arrays.fill(arr1,1); //arr1 모든 index값을 1로 초기

int[] arr2 = Arrays.copyOf(arr1, 3);
int[] arr2 = Arrays.copyOfRange(arr1, 2, 4);

Arrays.sort(arr);
Arrays.asList(arr);
Arrays.toString(arr);

//Collection

//ArrayList
ArrayList list = new ArrayList(); //타입 미설정 Object로 선언된다.
ArrayList<Student> members = new ArrayList<Student>(); //타입설정 Student객체만 사용가능
ArrayList<Integer> num = new ArrayList<Integer>(); //타입설정 int타입만 사용가능
ArrayList<Integer> num2 = new ArrayList<>(); //new에서 타입 파라미터 생략가능
ArrayList<Integer> num3 = new ArrayList<Integer>(10); //초기 용량(capacity)지정
ArrayList<Integer> list2 = new ArrayList<Integer>(Arrays.asList(1,2,3)); //생성시 값추가

list.add(3); //값 추가
list.add(null); //null값도 add가능
list.add(1,10); //index 1뒤에 10 삽입
list.remove(1);  //index 1 제거
list.clear();  //모든 값 제거
list.size(); //list 크기
list.get(0); //0번 index출력

ArrayList<String> convertedArr = new ArrayList<>();
convertedArr.sort((num1, num2)-> (num1+num2).compareTo(num2+num1));

for(Integer i : list) { //for문을 통한 전체출력
    System.out.println(i);
}

Iterator iter = list.iterator(); //Iterator 선언 
while(iter.hasNext()){//다음값이 있는지 체크
    System.out.println(iter.next()); //값 출력
}
list.contains(1);  //list에 1이 있는지 검색 : true
list.indexOf(1); //1이 있는 index반환 없으면 -1


//LinkedList
LinkedList list = new LinkedList();//타입 미설정 Object로 선언된다.
LinkedList<Student> members = new LinkedList<Student>();//타입설정 Student객체만 사용가능
LinkedList<Integer> num = new LinkedList<Integer>();//타입설정 int타입만 사용가능
LinkedList<Integer> num2 = new LinkedList<>();//new에서 타입 파라미터 생략가능
LinkedList<Integer> list2 = new LinkedList<Integer>(Arrays.asList(1,2));//생성시 값추가

list.addFirst(1);//가장 앞에 데이터 추가
list.addLast(2);//가장 뒤에 데이터 추가
list.add(3);//데이터 추가
list.add(1, 10);//index 1뒤에 데이터 10 추가
list.removeFirst(); //가장 앞의 데이터 제거
list.removeLast(); //가장 뒤의 데이터 제거
list.remove(); //생략시 0번째 index제거
list.remove(1); //index 1 제거
list.clear(); //모든 값 제거
list.size()
list.contains(1)
list.indexOf(1)


//Stack
import java.util.Stack; //import
Stack<Integer> stack = new Stack<>(); //int형 스택 선언
Stack<String> stack = new Stack<>(); //char형 스택 선언

stack.push(1);     // stack에 값 1 추가
stack.push(2);     // stack에 값 2 추가
stack.push(3);     // stack에 값 3 추가
stack.pop();       // stack에 값 제거
stack.clear();     // stack의 전체 값 제거 (초기화)
stack.empty();     // stack이 비어있는제 check (비어있다면 true)
stack.contains(1) // stack에 1이 있는지 check (있다면 true)

//Queue
import java.util.LinkedList; //import
import java.util.Queue; //import
Queue<Integer> queue = new LinkedList<>(); //int형 queue 선언, linkedlist 이용
Queue<String> queue = new LinkedList<>(); //String형 queue 선언, linkedlist 이용
queue.add(1);     // queue에 값 1 추가
queue.add(2);     // queue에 값 2 추가
queue.offer(3);   // queue에 값 3 추가
queue.poll();       // queue에 첫번째 값을 반환하고 제거 비어있다면 null
queue.remove();     // queue에 첫번째 값 제거
queue.clear();      // queue 초기화
queue.peek();       // queue의 첫번째 값 참조


```



Map

- HashMap
  가장 일반적으로 사용하는 Map. HashTable을 사용, Key값에 해시함수를 적용하여 나온 index에 Value를 저장하는 식. 중복성을 허용하지 않으며, 순서가 없다는 것이 특징
- https://coding-factory.tistory.com/556
- TreeMap
  Red-Black Tree 자료구조를 이용한 Map이다. Tree 구조이기 때문에 어느 정도 순서를 보장한다.
- 레드 블랙 트리는 부모 노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여 데이터의 추가나 삭제 시 트리가 한쪽으로 치우쳐지지 않도록 균형을 맞추어줍니다.
- 객체를 저장하면 자동으로 정렬되는데, 키는 저장과 동시에 자동 오름차순으로 정렬되고 숫자 타입일 경우에는 값으로, 문자열 타입일 경우에는 유니코드로 정렬합니다. 성능이 조금 떨어짐
- https://coding-factory.tistory.com/557
- LinkedHashMap
  LinkedList로 구현된 HashMap이다. List로 구현되어있기 때문에 순서가 보장된다. 하지만 LinkedList 특성상 랜덤 접근에서 느릴 수 있다.



**Set**

- HashSet
  HashMap에서 Key값이 없는 자료형. 집합이라고 생각해도 무방하다. 값이 포함되어 있는지 아닌지만 관심이 있다. 순서를 보장하지 않으며, 중복값을 허용하지 않는다. Set중에는 가장 많이 사용된다.
- HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode()메소드를 호출해서 해시 코드를 얻어낸 다음 저장되어 있는 객체들의 해시 코드와 비교한 뒤 같은 해시 코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않습니다. 
- https://coding-factory.tistory.com/554?category=758267
- TreeSet
  Red-Black Tree 자료구조를 사용한 Set.
- 이진 탐색 트리는 추가와 삭제에는 시간이 조금 더 걸리지만 정렬, 검색에 높은 성능을 보이는 자료구조입니다. 그렇기에 HashSet보다 데이터의 추가와 삭제는 시간이 더 걸리지만 검색과 정렬에는 유리합니다
- https://coding-factory.tistory.com/555?category=758267
- LinkedHashSet
  LinkedList로 구현된 HashSet. 순서를 보장한다.



### 우선순위 큐

힙 구조를 통해 만듬



**최대값 또는 최솟값**을  빠르게 찾아내기 위해 **완전이진트리**(모든 노드의 차수가 2이면서 마지막 레벨을 제외한 모든 노드가 채워져 있음, 왼쪽부터 채워져있음) 형태로 만들어진 자료구조

부모 노드는 항상 자식 노드보다 우선순위가 높다

이 조건을 만족하면서 완전 이진트리 형태로 채워 나가는 것

배열로 구현시 부모 노드 = 자식 노드 /2 로 편하게 구현 가능



**1.** 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조 (큐에 들어가는 원소는 비교가 가능한 기준이 있어야함) 

**2.** 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있음  

**3.** 내부구조가 힙으로 구성되어 있기에 시간 복잡도는 O(NLogN)

**4.** 응급실과 같이 우선순위를 중요시해야 하는 상황에서 쓰임



구현

```java
//add
//배열의 맨 마지막에 원소를 놓고 부모 노드를 찾아가면서 부모 노드가 삽입 노드보다 작을 때 까지 교환하면서 올라감

public void add(E value) {
		
	// 배열 용적이 꽉 차있을 경우 용적을 두 배로 늘려준다. 
	if(size + 1 == array.length) {
		resize(array.length * 2);
	}
		
	while(idx > 1) {
		int parent = idx / 2;	// 삽입노드의 부모노드 인덱스 구하기
		Object parentVal = array[parent];	// 부모노드의 값
		
		// 타겟 노드 값이 부모노드보다 크면 반복문 종료
		if(comp.compare(target, (E) parentVal) >= 0) {
			break;
		}
			
		/*
		 * 부모노드가 타겟노드보다 크므로
		 * 현재 삽입 될 위치에 부모노드 값으로 교체해주고
		 * 타겟 노드의 위치를 부모노드의 위치로 변경해준다. 
		 */
		array[idx] = parentVal;
		idx = parent;
	}
		
	// 최종적으로 삽입될 위치에 타겟 노드 값을 저장해준다.
	array[idx] = target;
	size++;	// 정상적으로 재배치가 끝나면 사이즈를 증가
}
//remove
//최상위 노드를 제거해 반환하고 가장 하위에 노드를 맨 위로 올린다음 자기 위치를 찾아 나간다

```

```java
import java.util.PriorityQueue; //import

//int형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();

//int형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());

int[] scoville
PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());
PriorityQueue<Integer> pq =
    new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

priorityQueue.add(1);  
priorityQueue.poll(); 
priorityQueue.clear();
priorityQueue.peek();       // priorityQueue에 첫번째 값 참조 = 1

PriorityQueue<T> pq = new PriorityQueue<>(initialCapacity, (e1, e2) -> { return e1.compareTo(e2); });
PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);
```





### DFS, BFS

```java
public static void dfs(int node, boolean[] visited) {
    if(visited[node]) return;

    visited[node] = true;

    for(int nextNode:nodeList[node]) {
        dfs(nextNode, visited, sb);
    }
}


public static void bfs(int node, boolean[] visited) {
    Queue<Integer> queue = new LinkedList<Integer>();

    queue.offer(node);

    while(!queue.isEmpty()) {
        node = queue.poll();

        if(visited[node]) continue;

        visited[node] = true;

        for(int nextNode:nodeList[node]) {
            queue.add(nextNode);
        }
    }
}

public class Main {

    static int Map[][];
    static boolean[] Visited;
    static int N,M,V;

    public static void dfs(int i){
        Visited[i] = true;
        System.out.print(i+" ");
        for(int j = 1; j < N+1; j++){
            if(Map[i][j] == 1 && Visited[j] == false){
                dfs(j);
            }
        }
    }

    public static void bfs(int i){
        Queue<Integer> q = new LinkedList<Integer>();
        q.offer(i);
        Visited[i] = true;

        while(!q.isEmpty()){
            int temp = q.poll();
            System.out.print(temp+" ");

            for(int k = 1; k <= N; k++){
                if(Map[temp][k] == 1 && Visited[k] == false){
                    q.offer(k);
                    Visited[k] = true;
                }
            }
        }
    }
}
```



### 기타 template

```java

//HashMap
HashMap<String,Integer> hashMap = new HashMap<String,Integer>();

hashMap.containsKey(1);
hashMap.get(1); //key로 찾기
hashMap.put(1, "사과"); //key 에 넣기
hasnMap.remove(1); //key값 제거
hashMap.clear(); //모든 값 제거

hashMap.getOrDefault(player, 0)
hashMap.put(player, hashMap.getOrDefault(player, 0) + 1);

//순회
//entrySet() 활용
for (Entry<Integer, String> entry : map.entrySet()) {
    System.out.println("[Key]:" + entry.getKey() + " [Value]:" + entry.getValue());
}

//KeySet() 활용
for(Integer i : map.keySet()){ //저장된 key값 확인
    System.out.println("[Key]:" + i + " [Value]:" + map.get(i));
}

//interator
Iterator<Entry<Integer, String>> entries = map.entrySet().iterator();
while(entries.hasNext()){
    Map.Entry<Integer, String> entry = entries.next();
    System.out.println("[Key]:" + entry.getKey() + " [Value]:" +  entry.getValue());
}


//HashSet
//HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode()메소드를 호출해서 해시 코드를 얻어낸 다음 저장되어 있는 객체들의 해시 코드와 비교한 뒤 같은 해시 코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않습니다. 문자열을 HashSet에 저장할 경우, 같은 문자열을 갖는 String객체는 동일한 객체로 간주되고 다른 문자열을 갖는 String객체는 다른 객체로 간주되는데, 그 이유는 String클래스가 hashCode()와 equals() 메소드를 재정의해서 같은 문자열일 경우 hashCode()의 리턴 값을 같게, equals()의 리턴 값은 true가 나오도록 했기 때문입니다.


HashSet<Integer> set1 = new HashSet<Integer>();//HashSet생성
HashSet<Integer> set2 = new HashSet<>();//new에서 타입 파라미터 생략가능
HashSet<Integer> set3 = new HashSet<Integer>(set1);//set1의 모든 값을 가진 HashSet생성
HashSet<Integer> set4 = new HashSet<Integer>(10);//초기 용량(capacity)지정
HashSet<Integer> set5 = new HashSet<Integer>(10, 0.7f);//초기 capacity,load factor지정
HashSet<Integer> set6 = new HashSet<Integer>(Arrays.asList(1,2,3));//초기값 지정
set.add(1); //값 추가
set.add(2);
set.add(3);
set.remove(1);//값 1 제거
set.clear();//모든 값 제거
set.size();
set.contains(1);
Iterator iter = set.iterator();	// Iterator 사용
while(iter.hasNext()) {//값이 있으면 true 없으면 false
    System.out.println(iter.next());
}
//중복 제거
HashSet<String> hashSet = (HashSet<String>) Arrays.stream(phone_book).collect(Collectors.toSet());

//Map -> List -> 정렬 
List<Map.Entry<Integer, Integer>> list = new LinkedList<>(genreMap.entrySet());

Collections.sort(list, new Comparator<Map.Entry<Integer, Integer>>() {
    @Override
    public int compare(Map.Entry<Integer, Integer> o1, Map.Entry<Integer, Integer> o2) {
        if (o1.getValue() > o2.getValue()) {
            return -1;
        }
        else{
            return 1;
        }
    }
});

//테크닉
//리스트, 배열 출력방법
queue.stream().forEach(temp -> System.out.println(temp));
System.out.println(Arrays.toString(answer));

//정수 나눗셈시 나머지가 잘릴 수 있다
int days = (int)Math.ceil((100 - progresses[i]) / (double)speeds[i]);

//변환
//int 배열 Integer 배열
int a[] = {1,2,3,4};
Integer b[] = Arrays.stream(a).boxed().toArray(Integer[]::new); 

//Integer 배열 int 배열
Integer a[] = {1,2,3,4};
int b[] = Arrays.stream(a).mapToInt(Integer::intValue).toArray(); 

//Integer 리스트를 int 배열로 변경방법
int[] answer = list.stream().mapToInt(Integer::intValue).toArray();

// To boxed list
List<Integer> you  = Arrays.stream( data ).boxed().collect( Collectors.toList() );
List<Integer> like = IntStream.of( data ).boxed().collect( Collectors.toList() );

//String 배열을 리스트로 리스트를 배열로
List<String> arrayList = Arrays.asList("a","b","C");
String[] array = arrayList.toArray(arrayList);

//int list to LinkedList
Queue<Integer> queue = (LinkedList<Integer>) Arrays.stream(priorities).boxed().collect(Collectors.toList());
Queue<Integer> queue = 
    new LinkedList<Integer>(Arrays.stream(truck_weights).boxed().collect(Collectors.toList()));

```



