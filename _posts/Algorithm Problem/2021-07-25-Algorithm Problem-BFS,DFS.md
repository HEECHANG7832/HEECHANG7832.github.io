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


### 네트워크
```java
class Solution {
    int[] checked;

    public int solution(int n, int[][] computers) {
        int answer = 0;

        checked = new int[n];

        int netNumber = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++) {
                if ((computers[i][j] == 1 || computers[j][i] == 1) && checked[i] == 0) { //연결되어 있으면
                    //연결된 상태에서 다른애랑 같은 netNumber
                    checked[i] = ++netNumber;
                    dfs(computers, i, netNumber);
                }
            }
        }

        Arrays.stream(checked).forEach(System.out::println);

        int count = 0;
        for (int value : checked) {
            if (value == 0) {
                count++;
            }
        }

        answer = count + netNumber;
        System.out.println(answer);
        return answer;
    }

    public void dfs(int[][] computers, int i, int netNumber){
        //다음 노드에서 연결된 다른 노드 같은 netNumber
        for(int j = 0; j < computers[i].length; j++){
            if(checked[j] == 0 && (computers[i][j] == 1 || computers[j][i] == 1)){ //탐색한적 없는 노드
                checked[j] = netNumber;
                dfs(computers, j, netNumber);
            }
        }
    }
}

class Solution {
    public int solution(int n, int[][] computers) {
        int answer = 0;
        boolean[] chk = new boolean[n];
        for(int i = 0; i < n; i++) {
            if(!chk[i]) {
                dfs(computers, chk, i);
                answer++;
            }
        }
        return answer;
    }
    void dfs(int[][] computers, boolean[] chk, int start) {
        chk[start] = true;
        for(int i = 0; i < computers.length; i++) {
            if(computers[start][i] == 1 && !chk[i]) {
                dfs(computers, chk, i);
            }
        }
    }
}
```
