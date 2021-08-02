---
layout: post
title: "알고리즘 문제풀이 정렬"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### K번째 수

```java
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
       int[] answer = {};

        answer = new int[commands.length];
        int index = 0;
        int[] temp = {};
        for(int i = 0; i < commands.length; i++){

            int start = commands[i][0] - 1;
            int end = commands[i][1];
            int k = commands[i][2] - 1;

            temp = Arrays.copyOfRange(array, start, end);

            Arrays.sort(temp);
            answer[index++] = temp[k];
        }

        //Arrays.stream(answer).forEach(System.out::println);

        return answer;
    }
}
```
