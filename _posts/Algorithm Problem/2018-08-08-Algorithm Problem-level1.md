







```java

class Solution {
  public String solution(String phone_number) {
     char[] ch = phone_number.toCharArray();
     for(int i = 0; i < ch.length - 4; i ++){
         ch[i] = '*';
     }
     return String.valueOf(ch);
  }
}
```



```java
public int getMean(int[] array) {
    return (int) Arrays.stream(array).average().orElse(0);
}

class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        
        OptionalDouble op = IntStream.of(arr).average();
                answer = op.getAsDouble();
        
        return answer;
    }
}

```



### 최대공약수 최소공배수

```java

class Solution {
    public long[] solution(int n, int m) {
        long[] answer = new long[2];
        
        if(n > m){
            int temp = n;
            n = m;
            m = temp;
        }
        
        long max = 0;
        long min = 0;
        
        for(int i = 1; i <= n; i++){
            if(n % i == 0 && m % i == 0){
                max = i;
            }
        }
        
        min = m * n / max;
        
        answer[0] = max;
        answer[1] = min;
        
        
        return answer;
    }
}
```





### 신규 아이디 추천

```JAVA
    static class Solution {
        public String solution(String new_id) {
            String answer = "";

            //1
            new_id = new_id.toLowerCase();
            System.out.println(new_id);

            //2
            StringBuilder sb = new StringBuilder();
            for(int i = 0; i < new_id.length(); i++){
                if(('a' <= new_id.charAt(i) && 'z' >= new_id.charAt(i)) ||
                        ('0' <= new_id.charAt(i) && '9' >= new_id.charAt(i)) ||
                        '-' == new_id.charAt(i) ||
                        '.' == new_id.charAt(i) ||
                        '_' == new_id.charAt(i)){
                    sb.append(new_id.charAt(i));
                }
            }
            System.out.println(sb.toString());
            new_id = sb.toString();
            sb.setLength(0);

            //3
            boolean flag = true;
            for(int i = 0; i < new_id.length(); i++){
                if(new_id.charAt(i) == '.'){
                    if(flag){
                        sb.append(".");
                        flag = false;
                    }else {
                        continue;
                    }
                }else {
                    flag = true;
                    sb.append(new_id.charAt(i));
                }
            }
            System.out.println(sb.toString());
            new_id = sb.toString();

            //4
            if(new_id.length() == 1){
                if(new_id.charAt(0) == '.'){
                    new_id = "";
                }
            }else{
                if(new_id.charAt(0) == '.'){
                    new_id = new_id.substring(1, new_id.length() - 1);
                }
                if(new_id.charAt(new_id.length() - 1) == '.'){
                    new_id = new_id.substring(0, new_id.length() - 1);
                }
            }
            System.out.println(new_id);

            //5
            if(new_id.isEmpty()){
                new_id = "a";
            }

            System.out.println(new_id);


            //6
            if(new_id.length() >= 16){
                new_id = new_id.substring(0, 15);
                if (new_id.charAt(new_id.length() - 1) == '.') {
                    new_id = new_id.substring(0, new_id.length() - 1);
                }
            }

            System.out.println(new_id);

            //7
            sb.setLength(0);
            if(new_id.length() <= 2){
                int i = 0;
                while(sb.length() != 3){
                    if(i == new_id.length() - 1){
                        sb.append(new_id.charAt(i));
                    }else{
                        sb.append(new_id.charAt(i)); i++;
                    }
                }
                new_id = sb.toString();
            }

            System.out.println(new_id);

            answer = new_id;
            return answer;
        }
    }

class Solution {
    public String solution(String new_id) {
        String answer = new_id.toLowerCase(); // 1단계

        answer = answer.replaceAll("[^-_.a-z0-9]", ""); // 2단계
        answer = answer.replaceAll("[.]{2,}", "."); // 3단계
        answer = answer.replaceAll("^[.]|[.]$", "");    // 4단계
        
        if (answer.equals("")) {    // 5단계
            answer += "a";
        }

        if (answer.length() >= 16) {     // 6단계
            answer = answer.substring(0, 15);
            answer = answer.replaceAll("[.]$","");
        }

        if (answer.length() <= 2) {  // 7단계
            while (answer.length() < 3) {
                answer += answer.charAt(answer.length()-1);
            }
        }

        return answer;
    }
}


```



### 제일 작은 수 제거

```java
class Solution {
  public int[] solution(int[] arr) {
      if (arr.length <= 1) return new int[]{ -1 };
      int min = Arrays.stream(arr).min().getAsInt();
      return Arrays.stream(arr).filter(i -> i != min).toArray();
  }
}
```

