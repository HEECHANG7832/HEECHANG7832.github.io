---
layout: post
title: "알고리즘 문제풀이 정렬"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### K번째 수

```java
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
       int[] answer = {};

        answer = new int[commands.length];
        int index = 0;
        int[] temp = {};
        for(int i = 0; i < commands.length; i++){

            int start = commands[i][0] - 1;
            int end = commands[i][1];
            int k = commands[i][2] - 1;

            temp = Arrays.copyOfRange(array, start, end);

            Arrays.sort(temp);
            answer[index++] = temp[k];
        }

        //Arrays.stream(answer).forEach(System.out::println);

        return answer;
    }
}
```
<<<<<<< HEAD


### 가장 큰 수

```java
import java.util.*;
import java.util.stream.Collectors;


class Solution {
    public String solution(int[] numbers) {
        String answer = "";

        List<Integer> temp = (ArrayList<Integer>)Arrays.stream(numbers).boxed().collect(Collectors.toList());

        Collections.sort(temp, new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {

                //자리수를 같게 만들어준다
                int alength = (int)( Math.log10(a)+1 );
                int blength = (int)( Math.log10(b)+1 );

                while (alength != blength) {
                    if(alength > blength){
                        int temp = b % 10; //1의자리
                        b = b * 10 + temp;
                        blength = (int)( Math.log10(b)+1 );
                    }else{
                        int temp = a % 10; //1의자리
                        a = a * 10 + temp;
                        alength = (int)( Math.log10(a)+1 );
                    }

                }

                return a > b ? -1 : 1;
            }
        });

        //temp.stream().forEach(System.out::println);

        StringBuffer sb = new StringBuffer();
        for(int a : temp){
            sb.append(Integer.valueOf(a));
        }
        //System.out.println(sb);
        answer = sb.toString();
        return answer;
    }
}
```
=======
>>>>>>> 3ab0e280c315089124a5b82f085f60aac793a4bd
