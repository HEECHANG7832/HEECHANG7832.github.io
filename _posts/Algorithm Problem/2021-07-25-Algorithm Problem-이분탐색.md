### 입국심사

```java
class Solution {
        public long solution(int n, int[] times) {
            long answer = 0;

            Arrays.sort(times);

            int left = 1;      // 최소 시간 1분
            int right = n * times[times.length-1];  // 최대 시간
            int middle ;
            int count ;

            while (left <= right) {
                middle = (int)Math.floor((left + right)/2); // 최대값에서 반을 나눈다.

                count = 0;
                for (int i : times) {
                    count += middle / i;
                }

                if(count > n) { // 제한인원보다 더 많이 검사 할 수 있으면
                    right = middle - 1; // middle값에서 - 1
                } else if(count < n) {
                    left = middle + 1;
                }
                if(count == n) { // 제한인원과 맞다면 시간을 리턴
                    return middle ;
                }
            }

            return answer;
        }
    }
```







### 징검다리

```java
import java.util.Arrays;

class Solution {
    public int solution(int distance, int[] rocks, int n) {
        Arrays.sort(rocks);
        int[] subDistances = new int[rocks.length+1];
        subDistances[0]=rocks[0];
        subDistances[rocks.length]=distance-rocks[rocks.length-1];
        for(int i=1; i<rocks.length; i++) subDistances[i] = rocks[i]-rocks[i-1];
        int left = 1;
        int right = distance;
        int mid = (left+right)/2;
        int current, removed;
        while(right-left > 1){
            removed = 0;
            current = 0;
            for(int i=0; i< subDistances.length; i++){
                current += subDistances[i];
                if(current < mid) removed++;
                else current = 0;
            }
            if(removed > n) right = mid;
            else left = mid;
            mid = (left+right)/2;
        }
        return mid;
    }
}
```

