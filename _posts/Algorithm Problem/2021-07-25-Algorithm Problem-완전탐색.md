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

### 소수찾기
```java
class Solution {

    HashSet<Integer> Set = new HashSet<>();
    StringBuffer sb = new StringBuffer("");
    Boolean[] check = new Boolean[7];

    public void findAllNumbers(int depth, int limit, String numbers){
        if(depth == limit) return;

        for(int i = 0 ; i < limit ; i++){
            if(!check[i]){
                check[i] = true;
                sb.append(numbers.charAt(i));
                Set.add(Integer.valueOf(sb.toString()));
                findAllNumbers(depth+1, limit, numbers);
                sb.deleteCharAt(sb.length() - 1);
                check[i] = false;
            }
        }
    }
    public int solution(String numbers) {
        int answer = 0;

        Arrays.fill(check, false);

        int size = numbers.length();

        findAllNumbers(0, size, numbers);

        //Set.stream().forEach(System.out::println);

        for(Integer a : Set){
            if(a == 1 || a == 0) continue;

            int num = (int)Math.sqrt(a);

            Boolean isDivided = false;
            for(int i = 2 ; i <= num ; i++){
                if(a % i == 0) isDivided = true;
            }
            if(!isDivided) answer++;
        }


        //System.out.println(answer);



        return answer;
    }
}

import java.util.HashSet;
class Solution {
    public int solution(String numbers) {
        HashSet<Integer> set = new HashSet<>();
        permutation("", numbers, set);
        int count = 0;
        while(set.iterator().hasNext()){
            int a = set.iterator().next();
            set.remove(a);
            if(a==2) count++;
            if(a%2!=0 && isPrime(a)){
                count++;
            }
        }        
        return count;
    }

    public boolean isPrime(int n){
        if(n==0 || n==1) return false;
        for(int i=3; i<=(int)Math.sqrt(n); i+=2){
            if(n%i==0) return false;
        }
        return true;
    }

        public void permutation(String prefix, String str, HashSet<Integer> set) {
        int n = str.length();
        //if (n == 0) System.out.println(prefix);
        if(!prefix.equals("")) set.add(Integer.valueOf(prefix));
        for (int i = 0; i < n; i++)
          permutation(prefix + str.charAt(i), str.substring(0, i) + str.substring(i+1, n), set);

    }

}


```


### 카펫
```java
class Solution {
    public int[] solution(int brown, int yellow) {
        int[] answer = new int[2];

        int x = 0;
        int y = 0;

        int value = (brown - 4) / 2;

        for (int i = value - 1; i > 0; i--) {
            if(i * (value - i) == yellow){
                x = i;
                y = value - i;
                break;
            }
        }

        answer[0] = x + 2;
        answer[1] = y + 2;

        return answer;
    }
}

import java.util.*;
class Solution {
    public int[] solution(int brown, int red) {
        int a = (brown+4)/2;
        int b = red+2*a-4;
        int[] answer = {(int)(a+Math.sqrt(a*a-4*b))/2,(int)(a-Math.sqrt(a*a-4*b))/2};
        return answer;
    }
}

```
