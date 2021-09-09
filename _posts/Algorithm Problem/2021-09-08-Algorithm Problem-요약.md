# JAVA 알고리즘 templet

### 시간복잡도

최고차항의 차수가 BioO가 된다

주어진 함수가 아무리 나빠도 비교하는 함수와 같거나 좋다

위로 갈수록 간단하고, 아래로 갈수록 복잡해지며, \log nlog*n*은 \log_2nlog2*n*을 뜻한다.


### java 기본
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

String

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

 ```JAVA

import java.util.Arrays;

public class Sort{
    public static void main(String[] args)  {
        int arr[] = {4,23,33,15,17,19};
        
        Arrays.sort(arr);
        Arrays.sort(arr, 0, 4); // 0,1,2,3 요소만
        Arrays.sort(arr,Collections.reverseOrder()); //내림차순



    }
}

//Custom
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

//버블정렬

//선택정렬

//삽입정렬

//Q정렬

//합병정렬

 ```


### DFS BFS

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



### 우선순위 큐

**1.** 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조 (큐에 들어가는 원소는 비교가 가능한 기준이 있어야함) 

**2.** 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있음  

**3.** 내부구조가 힙으로 구성되어 있기에 시간 복잡도는 O(NLogN)

**4.** 응급실과 같이 우선순위를 중요시해야 하는 상황에서 쓰임

```java
import java.util.PriorityQueue; //import

//int형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();

//int형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());

PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());
PriorityQueue<Integer> pq= new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

priorityQueue.add(1);  
priorityQueue.poll(); 
priorityQueue.clear();
priorityQueue.peek();       // priorityQueue에 첫번째 값 참조 = 1
```


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


//테크닉
//중복 제거
HashSet<String> hashSet = (HashSet<String>) Arrays.stream(phone_book).collect(Collectors.toSet());

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

// To boxed list
List<Integer> you  = Arrays.stream( data ).boxed().collect( Collectors.toList() );
List<Integer> like = IntStream.of( data ).boxed().collect( Collectors.toList() );

//Integer 리스트를 int 배열로 변경방법
int[] answer = list.stream().mapToInt(Integer::intValue).toArray();

//String 배열을 리스트로 리스트를 배열로
List<String> arrayList = Arrays.asList("a","b","C");
String[] array = arrayList.toArray(arrayList);


//int list to LinkedList
Queue<Integer> queue = (LinkedList<Integer>) Arrays.stream(priorities).boxed().collect(Collectors.toList());

Queue<Integer> queue = new LinkedList<Integer>(Arrays.stream(truck_weights).boxed().collect(Collectors.toList()));

```






