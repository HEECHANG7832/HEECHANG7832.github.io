---
layout: post
title: "알고리즘 문제풀이 탐욕법"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 체육복

```java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {

        Arrays.sort(lost);
        Arrays.sort(reserve);

        for(int i=0; i<lost.length; i++) {
          for(int j=0; j<reserve.length; j++) {
              if(lost[i]==reserve[j]) {
                  lost[i] = -1;
                  reserve[j] = -1;
                  break;
              }
          }
        }


        List<Integer> lostList = (ArrayList<Integer>)Arrays.stream(lost).boxed().map(a -> a > 0).collect(Collectors.toList());

        for(int i = 0; i < reserve.length; i++)
        {
            for(Integer j : lostList){
                if(reserve[i] != -1){
                  if(reserve[i] - 1 <= j && reserve[i] + 1 >= j){
                      lostList.remove(j);
                      answer++;
                      break;
                  }
                }
            }
        }
        int answer = n - lostList.length;


        return answer;
    }
}
```
