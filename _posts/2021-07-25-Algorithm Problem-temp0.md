---
layout: post
title: "알고리즘 문제풀이 스택, 큐"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---


### 기능개발


```java
import java.util.*;
import java.lang.Math;


class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        List<Integer> list = new ArrayList();
        List<Integer> queue = new ArrayList();

        for(int i = 0; i < progresses.length; i++){
            int days = (int)Math.ceil((100 - progresses[i]) / (double)speeds[i]);
            queue.add(days);
        }

        //queue.stream().forEach(temp -> System.out.println(temp));

        //출력

        int count = 1;
        int today = queue.remove(0);

        while(queue.size() != 0){
            int value = queue.remove(0);

            if(value <= today){
                count++;
            }else{
                list.add(count);
                count = 1;
                today = value;
            }
        }
        list.add(count);


        int[] answer = list.stream().mapToInt(Integer::intValue).toArray();
        //System.out.println(Arrays.toString(answer));
        return answer;
    }
}

import java.util.ArrayList;
import java.util.Arrays;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] dayOfend = new int[100];
        int day = -1;
        for(int i=0; i<progresses.length; i++) {
            while(progresses[i] + (day*speeds[i]) < 100) {
                day++;
            }
            dayOfend[day]++;
        }
        return Arrays.stream(dayOfend).filter(i -> i!=0).toArray();
    }
}
```


### 프린터

```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {


        int answer = 1;

        PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());

        //int list to LinkedList
        //Queue<Integer> queue = (LinkedList<Integer>) Arrays.stream(priorities).boxed().collect(Collectors.toList());

        for(int priority : priorities){
            pq.offer(priority);
        }

        while(!pq.isEmpty()){

            for (int i = 0; i < priorities.length; i++) {
                if (priorities[i] == pq.peek()) {
                    if (location == i) {
                       return answer;
                    }
                    pq.poll();
                    answer++;
                }
            }
        }
        return answer;

    }
}
```

### 다리를 지나는 트럭
