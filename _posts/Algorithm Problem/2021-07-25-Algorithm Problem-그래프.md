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
