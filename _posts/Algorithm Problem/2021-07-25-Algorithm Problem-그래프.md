---
layout: post
title: "알고리즘 문제풀이 그래프"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 가장 먼 노드

```java
class Solution {
    public int solution(int n, int[][] edge) {
        int[] dist = new int[n+1];
        boolean [][] map = new boolean[n+1][n+1];

        for (int i = 0; i < edge.length; i++) {
            map[edge[i][0]][edge[i][1]] = map[edge[i][1]][edge[i][0]] = true;
        }

        //BFS
        Queue<Integer> nodes = new LinkedList<Integer>();
        nodes.add(1);

        int maxDist = 0;
        while(!nodes.isEmpty()) {
            int i = nodes.poll();

            for (int j = 2; j <= n; j++) {
                if( dist[j] == 0 && map[i][j] ) {
                    dist[j] = dist[i] + 1;
                    nodes.add(j); //다음 탐색 위치
                    maxDist = Math.max(maxDist, dist[j]); //거리는 최대값만
                }
            }
        }

        // 가장 멀리 있는 노드가 몇 개인지 계산
        int count = 0;
        for (int d : dist) {
            if( maxDist == d )
                count ++;
        }

        return count;

    }
}
```



### 순위

```java
import java.util.*;
 
class Solution {
    
    public int solution(int n, int[][] results) {
        int INF = n * n + 1;
        
        int[][] floyd = new int[n + 1][n + 1];
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= n; j++) {
                floyd[i][j] = INF;
            }
            floyd[i][i] = 0;
        }
        
        for(int i = 0; i < results.length; i++) {
            floyd[results[i][0]][results[i][1]] = 1;
        }
        
        for(int k = 1; k <= n; k++) {
            for(int i = 1; i <= n; i++) {
                for(int j = 1; j <= n; j++) {
                    if(floyd[i][j] > floyd[i][k] + floyd[k][j]) {
                        floyd[i][j] = floyd[i][k] + floyd[k][j];
                    }
                }
            }
        }
        
        int result = 0;
        for(int i = 1; i <= n; i++) {
            boolean flag = true;
            for(int j = 1; j <= n; j++) {
                if(i != j && floyd[i][j] == INF && floyd[j][i] == INF) {
                    flag = false;
                    break;
                } 
            }
            if(flag) result++;
        }
        return result;
    }
}

```

