## 2021 상반기 Dev-matcing



### 다단계 칫솔 판매

```java
import java.util.*;
class Solution {
    
    public int[] solution(String[] enroll, String[] referral, String[] seller, int[] amount) {
        int[] answer = new int[enroll.length];
  
        Map<String, Integer> map = new HashMap<>();
        int[] Tree = new int[enroll.length];
        
        //make table
        for(int i = 0; i < enroll.length; i++){            
            map.put(enroll[i], i);
            if(referral[i].equals("-")){
                Tree[i] = -1; //center
            }else{
                Tree[i] = map.get(referral[i]);
            }
        }
        
        for(int i = 0; i < seller.length; i++){
            //수익을 낸사람 의 index
            int index = map.get(seller[i]);            
            
            //수익
            int a = amount[i] * 100;
            
            //수익 추가
            if(a <= 9){
                answer[index] += a;
                continue;
            }else{
                answer[index] += a - a / 10;
                a /= 10;    
            }
            
            //부모
            int parentIndex = Tree[index];
            
            do{              

                //부모가 센터면 갑저장 x
                if(parentIndex == -1){
                    continue;
                }else{
                    if(a <= 9){
                        answer[parentIndex] += a;   

                    }else{
                        answer[parentIndex] += a - a / 10;   

                    }
                }
                //다음 부모에게
                a /= 10;
                parentIndex = Tree[parentIndex];

            }
            while(parentIndex != -1 && a != 0);            
        }
        
        
        
        return answer;
    }
}
```



### 로또의 최고 순위와 최저 순위

```java
import java.util.*;

class Solution {
    public int[] solution(int[] lottos, int[] win_nums) {
        int[] answer = new int[2];
        
        Arrays.sort(lottos);
        Arrays.sort(win_nums);
        
        int blind = 0;
        int[] check = new int[6];
        for(int i = 0; i < lottos.length; i++){
            
            if(lottos[i] == 0){
                blind++;
                continue;
            }
            
            for(int j = 0; j < win_nums.length; j++){
                if(lottos[i] == win_nums[j]){
                    check[j] = 1; //맞음
                }
            }
        }
        
        int count = 0;
        for(int i = 0; i < 6; i++){
            if(check[i] == 1){
                count++;
            }
        }
        if(count == 0 && blind == 0){
            answer[0] = 6;
        }else{
            answer[0] = 7 - (count + blind);    
        }
    
        answer[1] = 2 > count ? 6 : 7 - count;
        
        return answer;
    }
}
```





### 행렬 테두리 회전하기

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
```





