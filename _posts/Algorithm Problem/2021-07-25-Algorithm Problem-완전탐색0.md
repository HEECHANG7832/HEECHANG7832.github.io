---
layout: post
title: "알고리즘 문제풀이 탐욕법"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 탐욕법

```java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {

        Arrays.sort(lost);
        Arrays.sort(reserve);

        int answer = n - lost.length;
        List<Integer> lostList = (ArrayList<Integer>)Arrays.stream(lost).boxed().collect(Collectors.toList());

        for(int i = 0; i < reserve.length; i++)
        {
            for(Integer j : lostList){
                if(reserve[i] - 1 <= j && reserve[i] + 1 >= j){

                    lostList.remove(j);
                    answer++;
                    break;
                }
            }
        }


        return answer;
    }
}
```
