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

class Solution {
    public int[] solution(int[] answers) {
        int[] answer = {};

        int[] a = {1,2,3,4,5};
        int[] b = {2,1,2,3,2,4,2,5};
        int[] c = {3,3,1,1,2,2,4,4,5,5};

        int ac = 0;
        int bc = 0;
        int cc = 0;

        int ai = 0;
        int bi = 0;
        int ci = 0;
        for(int i = 0; i < answer.length; i++){
            if(answer[i] == a[ai++]){
                ac++;    
            }
            if(answer[i] == b[bi++]){
                bc++;    
            }
            if(answer[i] == c[ci++]){
                cc++;    
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

        ac bc cc


        return answer;
    }
}
```
