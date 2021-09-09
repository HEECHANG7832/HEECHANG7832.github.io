### JAVA Collection

https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html

Array와 ArrayList의 다른점.
둘 다 배열이라는 점은 동일하나, Array는 인덱스로 접근하는 반면, ArrayList는 메서드를 통해 접근한다(어짜피 Index로 호출한다는 점은 동일 하겠지만..). 또한 Array는 Object 뿐만 아니라 원시 형태(Primitive, 예를 들어 int, double 등)도 담을 수 있지만, Array는 Object형(Reference, 객체)만 담을 수 있다. 따라서 정수를 ArrayList에 넣을 경우 Integer형은 가능하지만 int형은 안 된다. 덧붙여서, Integer처럼 int와 같은 원시타입을 담을 수 있는 객체를 Wrapper Class라고 한다

- List

- 객체로 데이터를 다루기 때문에 적은양의 데이터만 쓸 경우 배열에 비해 차지하는 메모리가 커진다. 간단히 예로들어 primitive type인 Int는 4Byte를 차지한다. 반면에 Wraaper class인 Integer는 32bit JVM에선 객체의 헤더(8Byte), 원시 필드(4Byte), 패딩(4Byte)으로 '최소 16Byte를 차지한다. 거기에다가 이러한 객체데이터들을 다시 주소로 연결하기 때문에 16 + α 가 된다.

- - ArrayList
    자바의 Vector를 개선한, **배열**로 구현된 List이다. 그 말인 즉슨, 데이터가 저장된 순서가 같다는 말이다. 사실상 배열과 같은 자료구조이기 때문에, 리스트의 연산 자체의 수행시간 속도는 배열과 같다. 모든 종류의 객체를 저장하지만 저장 및 조회시 변환해야 하므로 성능이 좋지 않다. 제네릭을 사용해 개선

  - ```java
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

  - LinkedList
    다음 노드의 주소를 기억하고 있는 List로, 배열에 비해 삽입과 삭제가 간단하다. 그러나 탐색의 경우 첫 번째 노드부터 탐색해 나가야 하기 때문에 느리다. 

  - ```java
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

  - 

  - Stack

  - ```java
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

    - E[] array 는 불가  Object[] array로 선언해야됨

  - Queue

  - Queue<Integer> q = new LinkedList();

  - ```java
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

  - Vector

- Map

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

- Set

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

- 우선순위 큐

  - 힙 구조를 통해 만듬

    - **최대값 또는 최솟값**을  빠르게 찾아내기 위해 **완전이진트리**(모든 노드의 차수가 2이면서 마지막 레벨을 제외한 모든 노드가 채워져 있음, 왼쪽부터 채워져있음) 형태로 만들어진 자료구조

    - 부모 노드는 항상 자식 노드보다 우선순위가 높다

    - 이 조건을 만족하면서 완전 이진트리 형태로 채워 나가는 것

    - 배열로 구현시 부모 노드 = 자식 노드 /2 로 편하게 구현 가능

    - ```java
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

  - 