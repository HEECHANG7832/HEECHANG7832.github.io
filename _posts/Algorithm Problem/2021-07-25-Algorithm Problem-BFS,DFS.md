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

### 단어 변환
```java
class Solution {
    public boolean compare(String a, String b) {
        int count = 0;
        for (int i = 0; i < a.length(); i++) {
            if(a.charAt(i) != b.charAt(i)){
                count++;
            }
        }
        return count == 1 ? true : false;
    }

    String Target;
    int Count;
    boolean[] visited;
    public int solution(String begin, String target, String[] words) {

        Target = target;
        visited = new boolean[words.length];

        for (int i = 0; i < words.length; i++) {
            if(visited[i] == false && compare(words[i], begin)){
                visited[i] = true;
                dfs(words, words[i], 1);
                visited[i] = false;
            }
        }
        System.out.println(Count);
        return Count;
    }

    public void dfs(String[] words, String word, int count){

        if(word.equals(Target)){
            Count = count;
        }

        for(int i = 0; i < words.length; i++){
            if(visited[i] == false && compare(words[i], word)){ //단어가 1개 차이나면
                visited[i] = true;
                dfs(words, words[i], count + 1);
                visited[i] = false;
            }
        }
    }
}


class Solution {

    static class Node {
        String next;
        int edge;

        public Node(String next, int edge) {
            this.next = next;
            this.edge = edge;
        }
    }

    public int solution(String begin, String target, String[] words) {
        int n = words.length, ans = 0;

        // for (int i=0; i<n; i++)
        //  if (words[i] != target && i == n-1) return 0;

        Queue<Node> q = new LinkedList<>();


        boolean[] visit = new boolean[n];
        q.add(new Node(begin, 0));

        while(!q.isEmpty()) {
            Node cur = q.poll();
            if (cur.next.equals(target)) {
                ans = cur.edge;
                break;
            }

            for (int i=0; i<n; i++) {
                if (!visit[i] && isNext(cur.next, words[i])) {
                    visit[i] = true;
                    q.add(new Node(words[i], cur.edge + 1));
                }
            }
        }

        return ans;
    }

    static boolean isNext(String cur, String n) {
        int cnt = 0;
        for (int i=0; i<n.length(); i++) {
            if (cur.charAt(i) != n.charAt(i)) {
                if (++ cnt > 1) return false;
            }
        }

        return true;
    }    
}

```

```java
import java.io.*;

public class BOJ_4963 {

	static int[][] map;
	//static boolean[][] visited;
	static int[] dx = {0, 0, 1, 1, 1, -1, -1, -1};
	static int[] dy = {1, -1, 0, 1, -1, 0, 1, -1};
	static int count;
	
	static void func(int x, int y) {
		for(int i = 0 ; i < 8 ; i++) {
			int mx = x + dx[i];
			int my = y + dy[i];
			if(map[mx][my] == 1) {
				map[mx][my] = 0;
				func(mx, my);
			}
		}	
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		
		while(true) {
			count = 0;

			String[] arr = in.readLine().split(" ");
			int r = Integer.parseInt(arr[0]);
			int c = Integer.parseInt(arr[1]);

			if(r == 0 && c == 0)
				break;
			
			map = new int[c + 2][r + 2];

			for(int i = 1 ; i <= c ; i++) {
				arr = in.readLine().split(" ");
				for(int j = 1 ; j <= r; j++) {
					map[i][j] = Integer.parseInt(arr[j-1]);
				}
			}

			for(int i = 1 ; i <= c ; i++) {
				for(int j = 1 ; j <= r ; j++) {
					if(map[i][j] == 1) {
						count++;
						func(i, j);
					}
				}
			}
			System.out.println(count);
		}
	}
}
```



### 여행경로

```java
import java.util.*;
import java.io.*;

class Solution {
    static String[][] ticketsInfo;
    static boolean[] used;
    static List<String> list = new ArrayList<String>();
    static String[] answer = {};

    public String[] solution(String[][] tickets) {
        used = new boolean[tickets.length]; // 총 방문 횟수
        ticketsInfo = tickets;

        Arrays.sort(ticketsInfo, (o1, o2) -> {
           if(o1[0].equals(o2[0])) {
               return o1[1].compareTo(o2[1]);
           } else {
               return o1[0].compareTo(o2[0]);
           }
        });

        list.add("ICN");
        dfs("ICN", 0);
        return answer;
    }

    private void dfs(String start, int cnt) {
        if(answer.length>0) return; // 이미 알파벳이 더 빠른 정답이 나왔는데 dfs가 호출됐다면 return

        if(cnt == used.length) { // 모든 티켓을 다 사용했다면
            answer = new String[list.size()]; // answer 초기화
            for(int i=0; i<list.size(); i++) {
                answer[i] = list.get(i); // answer에 list 값 담기
            }
            return;
        }

        for(int i=0 ; i< ticketsInfo.length; i++) { // 모든 티켓을 살펴보면서
	        // 출발지가 start와 동일하고, 사용하지 않은 티켓이면
            if(!used[i] && ticketsInfo[i][0].equals(start)) { 
                used[i] = true; // i번째 티켓은 사용했다.
                list.add(ticketsInfo[i][1]); // i번째 티켓의 도착지 방문
                dfs(ticketsInfo[i][1], cnt +1); // 도착지는 출발지(start)가 된다, 다음 티켓 사용(cnt + 1)
                // return 된 후, 다른 경우의 수를 살펴보기 위해 아래 두 줄을 추가한다.
                used[i] = false; // i번째 사용 여부 false로 바꾸기
                list.remove(list.size()-1); // i번째 티켓의 도착지 제거
            }
        }
    }
}
```

