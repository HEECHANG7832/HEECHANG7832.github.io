---
layout: post
title: "알고리즘 문제풀이 힙"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

### 더 맵게

```java

import java.util.*;
import java.util.stream.Collectors;


class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;

        PriorityQueue<Integer> pq= new PriorityQueue<>(Arrays.stream(scoville).boxed().collect(Collectors.toList()));

        while(pq.peek() < K){

            int a = pq.poll();
            int b = pq.poll();
            pq.offer(a + b * 2);
            answer++;
            if(pq.size() == 1 && pq.peek() < K){
                answer = -1;
                break;
            }
        }



        return answer;
    }
}
```



### 디스크 컨트롤러

```java

import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {

	public int solution(int[][] jobs) {

		int answer = 0;
		int end = 0; // 수행되고난 직후의 시간
		int jobsIdx = 0; // jobs 배열의 인덱스
		int count = 0; // 수행된 요청 갯수

		// 원본 배열 오름차순 정렬 (요청시간 오름차순)
		Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);

		// 처리 시간 오름차순으로 정렬되는 우선순위 큐(Heap)
		PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);

		// 요청이 모두 수행될 때까지 반복
		while (count < jobs.length) {

			// 하나의 작업이 완료되는 시점(end)까지 들어온 모든 요청을 큐에 넣음
			while (jobsIdx < jobs.length && jobs[jobsIdx][0] <= end) {
				pq.add(jobs[jobsIdx++]);
			}

			// 큐가 비어있다면 작업 완료(end) 이후에 다시 요청이 들어온다는 의미
			// (end를 요청의 가장 처음으로 맞춰줌)
			if (pq.isEmpty()) {
				end = jobs[jobsIdx][0];

			// 작업이 끝나기 전(end 이전) 들어온 요청 중 가장 수행시간이 짧은 요청부터 수행
			} else {

				int[] temp = pq.poll();
				answer += temp[1] + end - temp[0];
				end += temp[1];
				count++;
			}
		}

		return (int) Math.floor(answer / jobs.length);
	}
}
```



### 이중 우선순위

```java
import java.util.*;
class Solution {
    public int[] solution(String[] operations) {
       int[] answer = new int[2];
        //String[] operations = {"I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"};

        PriorityQueue<Integer> pqMin = new PriorityQueue<>();
        PriorityQueue<Integer> pqMax = new PriorityQueue<>(Collections.reverseOrder());

        for (String s : operations) {
            String[] sp = s.split(" ");
            if(sp[0].equals("I")){
                pqMax.add(Integer.parseInt(sp[1]));
                pqMin.add(Integer.parseInt(sp[1]));
            }else {


                //최댓값 삭제
                if (sp[1].equals("-1")) {
                    pqMax.remove(pqMin.poll());

                    //최솟값 빼기
                } else {
                    pqMin.remove(pqMax.poll());
                }
            }
        }

        answer[0] = pqMax.peek() != null ? pqMax.peek() : 0;
        answer[1] = pqMin.peek() != null ? pqMin.peek() : 0;
        return answer;
    }
}
```

