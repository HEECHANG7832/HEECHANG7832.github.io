---
layout: post
title: "2021 상반기 dev-matching"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

```java
import java.util.*;
class Solution {
    public int[] solution(int rows, int columns, int[][] queries) {
        
        
        int[] answer = new int[queries.length];

        //init map
        int[][] map = new int[rows + 1][columns + 1];

        int value = 1;
        for(int i = 1; i <= rows; i++)
        {
            for(int j = 1; j <= columns; j++)
            {
                map[i][j] = value++;
            }
        }

        int count = 0;
        for(int i = 0; i < queries.length; i++)
        {
            //get query
            int[] query = queries[i];

            //change map
            int x1 = query[0];
            int y1 = query[1];
            int x2 = query[2];
            int y2 = query[3];

            List<Integer> list = new ArrayList<>();

            //2D deep copy
            int[][] tempMap = new int[rows + 1][columns + 1];
            for(int k=0; k<map.length; k++) {
                for(int j=0; j<map[k].length; j++) {
                    tempMap[k][j] = map[k][j];
                }
            }

            //y1 to y2
            for(int j = y1; j < y2; j++)
            {
                map[x1][j + 1] = tempMap[x1][j];
                list.add(tempMap[x1][j]);
            }
            //System.out.println(Arrays.deepToString(tempMap));

            //x1 to x2
            for(int j = x1; j < x2; j++)
            {
                map[j + 1][y2] = tempMap[j][y2];
                list.add(tempMap[j][y2]);
            }

            //y2 to y1
            for(int j = y2; j > y1; j--)
            {
                map[x2][j - 1] = tempMap[x2][j];
                list.add(tempMap[x2][j]);
            }

            //x2 to x1
            for(int j = x2; j > x1; j--)
            {
                map[j - 1][y1] = tempMap[j][y1];
                list.add(tempMap[j][y1]);
            }

            //System.out.println(Arrays.deepToString(map));


            //get min value
            int min = Collections.min(list);

            //System.out.println(min);

            answer[count++] = min;

            
        }
        //System.out.println(Arrays.toString(answer));
        
        return answer;
    }
}


class Solution {
    static int map[][];
    public int[] solution(int rows, int columns, int[][] queries) {
        int[] answer = new int[queries.length];

        map=new int[rows+1][columns+1];
        int idx=1;
        for(int i=1; i<=rows; i++){
            for(int j=1; j<=columns; j++){
                map[i][j]=idx++;
            }
        }
        for(int i=0; i<queries.length; i++){

            answer[i]=rotate(queries[i][0],queries[i][1],queries[i][2],queries[i][3]);

        }

        return answer;


    }static int rotate(int x1,int y1,int x2,int y2){
        int x=x1;
        int y=y1;
        int[]dx={0,-1,0,1};
        int[]dy={1,0,-1,0};
        int dir=3;
        int temp=map[x][y];
        int min=temp;
        while(true){
            if(x==x2&&y==y1){
                dir=0;
            }
             if(x==x2&&y==y2)dir=1;
             if(x==x1&&y==y2)dir=2;
            map[x][y]=map[x+dx[dir]][y+dy[dir]];
            x+=dx[dir];
            y+=dy[dir];
            min=Math.min(map[x][y],min);
            if(x==x1&&y==y1){
                map[x1][y1+1]=temp;
                break;
            }
        }

        return min;
    }
}


```