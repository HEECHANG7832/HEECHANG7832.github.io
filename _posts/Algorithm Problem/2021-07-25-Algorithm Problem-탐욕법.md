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
