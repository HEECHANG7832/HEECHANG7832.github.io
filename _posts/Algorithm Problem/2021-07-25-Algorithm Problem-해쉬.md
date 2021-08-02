---
layout: post
title: "알고리즘 문제풀이 해쉬"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### hashMap

```java

import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String,Integer> hashMap = new HashMap<String,Integer>();

        for(int i = 0; i < completion.length; i++){
            int count = 0;
            if(hashMap.containsKey(completion[i])){
                count = hashMap.get(completion[i]);
                hashMap.put(completion[i], count + 1);
            }else{
                hashMap.put(completion[i], 1);    
            }
        }

        for(int i = 0; i < participant.length; i++){
            int count = 0;
            if(hashMap.containsKey(participant[i])){
                count = hashMap.get(participant[i]);
                hashMap.put(participant[i], count - 1);
            }
        }

        for(int i = 0; i < participant.length; i++){
            if(hashMap.get(participant[i]) == null || hashMap.get(participant[i]) != 0){
                answer = participant[i];
                break;
            }
        }
        return answer;
    }
}


---

import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> hm = new HashMap<>();
        for (String player : participant) hm.put(player, hm.getOrDefault(player, 0) + 1);
        for (String player : completion) hm.put(player, hm.get(player) - 1);

        for (String key : hm.keySet()) {
            if (hm.get(key) != 0){
                answer = key;
            }
        }
        return answer;
    }
}
```




```java
import java.util.Arrays;

class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;

        Arrays.sort(phone_book);

        for(int i = 0; i < phone_book.length; i++){
            for(int j = i + 1; j < phone_book.length; j++){
                 if(phone_book[j].indexOf(phone_book[i]) == 0){
                     answer = false;
                 }
            }
        }

        return answer;
    }
}

Arrays.sort(phone_book); 하면  스트링은 정렬이 안




class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        int minLength = 99;

        for(String iter : phone_book){
            if(minLength > iter.length()){
                minLength = iter.length();
            }
        }

        for(int i = 0; i < phone_book.length; i++){
            phone_book[i] = phone_book[i].substring(0, minLength);
        }

        HashSet<String> hashSet = (HashSet<String>) Arrays.stream(phone_book).collect(Collectors.toSet());


        if(phone_book.length != hashSet.size()){
            return false;
        }

        return answer;
    }
}


import java.util.Arrays;

class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;

        Arrays.sort(phone_book);

		for(int i=0; i<phone_book.length-1; i++) {
			if(phone_book[i+1].startsWith(phone_book[i])){
				answer = false;
                break;
			}
		}

        return answer;
    }
}
```

### 프로그래머스 - 위장


```java

import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 0;

        HashMap<String, Integer> hashMap = new HashMap();

        for(String[] cloth : clothes){
            hashMap.put(cloth[1], hashMap.getOrDefault(cloth[1], 0) + 1);
        }

        answer = 1;
        for(String iter : hashMap.keySet()){ //저장된 key값 확인
            answer *= hashMap.get(iter) + 1;
        }

        answer--;

        return answer;
    }
}



import java.util.*;
import static java.util.stream.Collectors.*;

class Solution {
    public int solution(String[][] clothes) {
        return Arrays.stream(clothes)
                .collect(groupingBy(p -> p[1], mapping(p -> p[0], counting())))
                .values()
                .stream()
                .collect(reducing(1L, (x, y) -> x * (y + 1))).intValue() - 1;
    }
}



```
