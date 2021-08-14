---
layout: post
title: "알고리즘 문제풀이 BFS, DFS"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---


### 타겟 넘버
```java
class Solution {

    int[] Numbers;
    int Target;
    int Answer = 0;

    public int solution(int[] numbers, int target) {

        Numbers = Arrays.copyOf(numbers, numbers.length);

        Target = target;

        dfs(0, 0);

        System.out.println(Answer);
        return Answer;
    }

    public void dfs(int value, int index){

        if(index == Numbers.length){
            if(value == Target){
                Answer++;
                //System.out.println(value);
            }
            return;
        }

        dfs(value + Numbers[index], index + 1);
        dfs(value - Numbers[index], index + 1);


    }
}

class Solution {
    public int solution(int[] numbers, int target) {
        int answer = 0;
        answer = dfs(numbers, 0, 0, target);
        return answer;
    }
    int dfs(int[] numbers, int n, int sum, int target) {
        if(n == numbers.length) {
            if(sum == target) {
                return 1;
            }
            return 0;
        }
        return dfs(numbers, n + 1, sum + numbers[n], target) + dfs(numbers, n + 1, sum - numbers[n], target);
    }
}

```
