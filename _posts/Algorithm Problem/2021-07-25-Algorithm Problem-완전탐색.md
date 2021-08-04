---
layout: post
title: "알고리즘 문제풀이 완전탐색"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 모의고사

```java
import java.util.*;
import java.util.stream.*;

class Solution {
    public int[] solution(int[] answers) {
        int[] answer = {};

        int[] a = {1,2,3,4,5};
        int[] b = {2,1,2,3,2,4,2,5};
        int[] c = {3,3,1,1,2,2,4,4,5,5};

        int ai = 0;
        int bi = 0;
        int ci = 0;

        int count[] = new int[3];
        int maxCount = 0;
        for(int i = 0; i < answers.length; i++){
            if(answers[i] == a[ai++]){
                count[0]++;    
            }
            if(answers[i] == b[bi++]){
                count[1]++;    
            }
            if(answers[i] == c[ci++]){
                count[2]++;    
            }


            if(ai == a.length){
                ai = 0;
            }
            if(bi == b.length){
                bi = 0;
            }
            if(ci == c.length){
                ci = 0;
            }
        }
        maxCount = Arrays.stream(count).max().getAsInt();

        List<Integer> temp = new ArrayList();
        for(int i = 0; i < 3; i++){
            if(count[i] == maxCount){
                temp.add(i + 1);    
            }
        }

        answer = temp.stream().mapToInt(Integer::intValue).toArray();


        return answer;
    }
}

import java.util.ArrayList;
class Solution {
    public int[] solution(int[] answer) {
        int[] a = {1, 2, 3, 4, 5};
        int[] b = {2, 1, 2, 3, 2, 4, 2, 5};
        int[] c = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
        int[] score = new int[3];
        for(int i=0; i<answer.length; i++) {
            if(answer[i] == a[i%a.length]) {score[0]++;}
            if(answer[i] == b[i%b.length]) {score[1]++;}
            if(answer[i] == c[i%c.length]) {score[2]++;}
        }
        int maxScore = Math.max(score[0], Math.max(score[1], score[2]));
        ArrayList<Integer> list = new ArrayList<>();
        if(maxScore == score[0]) {list.add(1);}
        if(maxScore == score[1]) {list.add(2);}
        if(maxScore == score[2]) {list.add(3);}
        return list.stream().mapToInt(i->i.intValue()).toArray();
    }
}


```
