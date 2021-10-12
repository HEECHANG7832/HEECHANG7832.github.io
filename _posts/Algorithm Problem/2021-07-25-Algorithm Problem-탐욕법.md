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
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {

        Arrays.sort(lost);
        Arrays.sort(reserve);

        int count = 0;
        for(int i=0; i<lost.length; i++) {
            for(int j=0; j<reserve.length; j++) {
                if(lost[i]==reserve[j]) {
                    lost[i] = -1;
                    reserve[j] = -1;
                    count++;
                    break;
                }
            }
        }



        int answer = n - lost.length + count;

        for (int i = 0; i < lost.length; i++) {
            if(lost[i] != -1) {
                for (int j = 0; j < reserve.length; j++) {
                    if (reserve[j] != -1) {
                        if (lost[i] + 1 == reserve[j] || lost[i] - 1 == reserve[j]) {

                            reserve[j] = -1;
                            answer++;
                            break;
                        }
                    }
                }
            }
        }
        System.out.println(answer);
        return answer;
    }
}

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int[] people = new int[n];
        int answer = n;

        for (int l : lost)
            people[l-1]--;
        for (int r : reserve)
            people[r-1]++;

        for (int i = 0; i < people.length; i++) {
            if(people[i] == -1) {
                if(i-1>=0 && people[i-1] == 1) {
                    people[i]++;
                    people[i-1]--;
                }else if(i+1< people.length && people[i+1] == 1) {
                    people[i]++;
                    people[i+1]--;
                }else
                    answer--;
            }
        }
        return answer;
    }
}
```

### 큰 수 만들기

```java
class Solution {
    public String solution(String number, int k) {


        StringBuffer sb = new StringBuffer();
        //골라야 하는 갯수, 윈도우 사이즈
        for(int i = 0, index = -1; i < number.length() - k; i++){

            char max = 0;
            //스트링에서 현 위치부터 윈도우 사이즈만큼 최대값 고르고
            //다음 찾는 위치 변경
            for (int j = index + 1; j <= k + i; j++) {
                if (max < number.charAt(j)) {
                    index = j;
                    max = number.charAt(j);
                }
            }
            sb.append(max);
        }


        return sb.toString();
    }
}

import java.util.Stack;
class Solution {
    public String solution(String number, int k) {
        char[] result = new char[number.length() - k];
        Stack<Character> stack = new Stack<>();

        for (int i=0; i<number.length(); i++) {
            char c = number.charAt(i);
            while (!stack.isEmpty() && stack.peek() < c && k-- > 0) {
                stack.pop();
            }
            stack.push(c);
        }
        for (int i=0; i<result.length; i++) {
            result[i] = stack.get(i);
        }
        return new String(result);
    }
}
```


### 구명보트

```java


import java.util.Arrays;
import java.util.Collections;
class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;

        int[] check = new int[people.length];

        Integer[] p =  Arrays.stream(people).boxed().toArray( Integer[]::new );
        Arrays.sort(p, Collections.reverseOrder());

        //몸무게가 젤 큰 사람부터
        for (int i = 0; i < p.length; i++) {
            if(check[i] != -1){ //아직 안탄 사람만

                //한사람 태운다
                int l = limit - p[i];
                check[i] = -1;

                //그사람 다음 사람부터 최대 몸무게로 사람들을 1명 대려감
                for (int j = i + 1; j < p.length; j++) {
                    if(check[j] != -1 && p[j] <= l){
                        check[j] = -1; //태움
                        //System.out.println(j);
                        break;
                    }
                }
                answer++;
            }
        }
        System.out.println(answer);
        return answer;
    }
}

class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;

        int[] check = new int[people.length];

        Integer[] p =  Arrays.stream(people).boxed().toArray( Integer[]::new );
        Arrays.sort(p, Collections.reverseOrder());

        //몸무게가 큰 사람부터
        int j = p.length - 1;
        for (int i = 0; i < p.length; i++) {

            if(p[i] > limit / 2){
                int l = limit - p[i]; //남은 무게
                //무게 작은 순으로 데려갈 사람이 있는지
                if(p[j] <= l){
                    j--;
                }
                answer++;
            }else{
                answer += Math.ceil((double)(j - i + 1) / 2);
                break;
            }

        }

        System.out.println(answer);

        return answer;
    }
}

import java.util.Arrays;

class Solution {
    public int solution(int[] people, int limit) {
        Arrays.sort(people);
        int i = 0, j = people.length - 1;
        for (; i < j; --j) {
            if (people[i] + people[j] <= limit)
                ++i;
        }
        return people.length - i;
    }
}
```



### 조이스틱



```java
public int solution(String name) {
  int answer = 0;
  int len = name.length();

  // 제일 짧은 좌, 우 이동은 그냥 맨 오른쪽으로 이동할 때
  int min = len - 1;

  for (int i = 0; i < len; i++) {
    // 조이스틱 상, 하 이동
    char c = name.charAt(i);
    int mov = (c - 'A' < 'Z' - c + 1) ? (c - 'A') : ('Z' - c + 1);
    answer += mov;

    // 조이스틱 좌, 우 이동
    int nextIndex = i + 1;
    // 다음 단어가 A이고, 단어가 끝나기 전까지 nextIndex 증가
    while (nextIndex < len && name.charAt(nextIndex) == 'A')
      nextIndex++;

    min = Math.min(min, (i * 2) + len - nextIndex);
  }

  answer += min;

  return answer;
}
```



### 섬 연결하기

```JAVA
import java.util.*;

class Solution {
    
    static int[] parent;
    
    public int solution(int n, int[][] costs) {
        
        Arrays.sort(costs, (int[] c1, int[] c2) -> c1[2] - c2[2]);
        
        parent = new int[n];
        
        for(int i = 0; i < n; i++)
        {
            parent[i] = i;            
        }
        
        int total = 0;
        for(int[] edge : costs)
        {
            int from = edge[0];
            int to = edge[1];
            int cost = edge[2];
            
            int fromParent = findParent(from);
            int toParent = findParent(to);
            
            if(fromParent == toParent) continue;
            
            total += cost;
            parent[toParent] = fromParent;
        }
        return total;
    }
    
    private int findParent(int node){
        if(parent[node] == node) return node;
        return parent[node] = findParent(parent[node]);
    }
}
```



### 단속카메라

```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public int solution(int[][] routes) {
        int answer = 0;
        
        Arrays.sort(routes, new Comparator<int[]>() {
            @Override
            public int compare(int[] route1, int[] route2) {
                return route1[1] - route2[1];
            }
        });
        
        int cam = Integer.MIN_VALUE;
        
        for(int[] route : routes) {
            if(cam < route[0]) {
                // 현재 카메라의 위치가 route의 시작 지점보다 작으면
                // 새로운 cam을 route의 종료 지점에 설치한다
                cam = route[1];
                answer++;
            }
        }
        
        return answer;
    }
}
```

