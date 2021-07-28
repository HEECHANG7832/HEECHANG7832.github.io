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
            int days = (int)Math.ceil((100 - progresses[i]) / speeds[i]);
            queue.add(days);
        }

        //queue.stream().forEach(temp -> System.out.println(temp));

        //출력

        int count = 0;
        int today = queue.get(0);

        for(int a : queue){
            if(a <= today){
                count++;
            }else {
                list.add(count);
                count = 1;
                today = a;
            }
        }
        list.add(1);


        int[] answer = list.stream().mapToInt(Integer::intValue).toArray();
        //System.out.println(Arrays.toString(answer));

        return answer;
    }
}
```
