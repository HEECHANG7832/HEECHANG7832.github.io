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


import java.util.*;

class Solution {
    public String solution(int[] numbers) {
        String result = "";
        ArrayList<String> convertedArr = new ArrayList<>();
        for (int number : numbers) {
            convertedArr.add(String.valueOf(number));
        }
        convertedArr.sort((num1, num2)-> (num1+num2).compareTo(num2+num1));
        for (String s : convertedArr) {
            result = s + result;
        }
        while (result.length() > 1 && result.charAt(0) == '0'){
            result = result.substring(1);
        }
        return result;
    }
}
```



### h-index

```java
import java.util.*;

public class Solution{
    public static void main(String[] args) {

        int answer = 0;
        int[] citations = {9, 7, 6, 2, 1}; //3

        Integer citations1[] = Arrays.stream(citations).boxed().toArray(Integer[]::new);
        Arrays.sort(citations1, Collections.reverseOrder());


        //h번 이상 인용된 논문이 h편 이상 나머지가 h 이하

        for (int i = 0; i < citations1.length; i++) {

            if(i + 1 <= citations1[i]){
                answer = i + 1;
            }
        }
        System.out.println(answer);
    }
}

class Solution {
    public int solution(int[] citations) {
        Arrays.sort(citations);

        int max = 0;
        for(int i = citations.length-1; i > -1; i--){
            int min = (int)Math.min(citations[i], citations.length - i);
            if(max < min) max = min;
        }

        return max;
    }
}
```

