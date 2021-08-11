---
layout: post
title: "알고리즘 문제풀이 요약"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---


```java
//해쉬

HashMap<String,Integer> hashMap = new HashMap<String,Integer>();


hashMap.containsKey(completion[i])
hashMap.get(completion[i]);
hashMap.put(completion[i], count + 1);
hm.put(player, hm.getOrDefault(player, 0) + 1);

Arrays.sort(phone_book); 하면  스트링 숫자형태 은 정렬이 안됨

String[] phone_book
HashSet<String> hashSet = (HashSet<String>) Arrays.stream(phone_book).collect(Collectors.toSet());


//스택 큐
리스트, 배열 출력방법
queue.stream().forEach(temp -> System.out.println(temp));
System.out.println(Arrays.toString(answer));

정수 나눗셈시 나머지가 잘릴 수 있다
int days = (int)Math.ceil((100 - progresses[i]) / (double)speeds[i]);

Integer 리스트를 int 배열로 변경방법
int[] answer = list.stream().mapToInt(Integer::intValue).toArray();

String 배열을 리스트로 리스트를 배열로
List<String> arrayList = Arrays.asList("a","b","C");
String[] array = arrayList.toArray(arrayList);


PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());
PriorityQueue<Integer> pq= new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

//int list to LinkedList
Queue<Integer> queue = (LinkedList<Integer>) Arrays.stream(priorities).boxed().collect(Collectors.toList());
Queue<Integer> queue = new LinkedList<Integer>(Arrays.stream(truck_weights).boxed().collect(Collectors.toList()));


// To boxed array
Integer[] what = Arrays.stream( data ).boxed().toArray( Integer[]::new );
Integer[] ever = IntStream.of( data ).boxed().toArray( Integer[]::new );

// To boxed list
List<Integer> you  = Arrays.stream( data ).boxed().collect( Collectors.toList() );
List<Integer> like = IntStream.of( data ).boxed().collect( Collectors.toList() );
```
