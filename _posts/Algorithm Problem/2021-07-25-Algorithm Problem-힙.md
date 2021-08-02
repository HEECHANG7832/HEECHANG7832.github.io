---
layout: post
title: "알고리즘 문제풀이 힙"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 더 맵게

```java

import java.util.*;
import java.util.stream.Collectors;


class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;

        PriorityQueue<Integer> pq= new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

        while(pq.peek() < K){

            int a = pq.poll();
            int b = pq.poll();
            pq.offer(a + b * 2);
            answer++;
            if(pq.size() == 1 && pq.peek() < K){
                answer = -1;
                break;
            }
        }



        return answer;
    }
}
```
