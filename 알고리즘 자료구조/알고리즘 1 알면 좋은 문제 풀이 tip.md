# 알고리즘 1 알면 좋은 문제 풀이 tip

##  후위 표기식

<img src="https://user-images.githubusercontent.com/101400894/183328322-342c3e93-93e7-41ae-9df1-18785ffa0e63.png" alt="image" style="zoom:80%;" />



### 중위 연산자를 후위 연산자로 만들기

```java
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        Stack<Character> stack = new Stack<>();

        String str = br.readLine();

        StringBuilder sb = new StringBuilder();

        for(int i = 0; i<str.length(); i++){
            switch (str.charAt(i)){
                case '*' : case '+' : case '-' : case '/' :
                    while(!stack.isEmpty() && priority(stack.peek()) >= priority(str.charAt(i))){
                        sb.append(stack.pop());
                    }
                    stack.push(str.charAt(i));
                    break;
                case '(' :
                    stack.push(str.charAt(i));
                    break;
                case ')' :
                    while(!stack.isEmpty()){
                        if(stack.peek() == '(') {
                            stack.pop();
                            break;
                        }
                        sb.append(stack.pop());
                    }
                    break;
                default:
                    sb.append(str.charAt(i));
                    break;
            }
        }
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        System.out.println(sb);
    }

    public static int priority(char c){
        if(c == '*' || c == '/') return 2;
        else if(c == '+' || c == '-') return 1;
        else return 0;
    }
}
```

+ *, /가 있는 경우 한번 stack을 비우면서 연산자를 표시해야 함(우선순위를 나눌 필요가 있음)
+ (, )는 (가 시작되면 ) 나올때 까지 계속 받다가 한번에 표시
+ 연산자의 순서가 제일 앞에 있는것이 연산자 바로 앞의 숫자와 매핑
  + abc*+ -> a + b * c



### 후위 연산자 계산

```java
public class Main {
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int count = Integer.parseInt(br.readLine());
        String str = br.readLine();
        Stack<Double> stack = new Stack<>();
        double[] number = new double[count];
        for(int i = 0; i<number.length; i++){
            number[i] = Double.parseDouble(br.readLine()); //A부터 0번째 시작
        }

        double result = 0;

        for(int i = 0; i < str.length(); i++){
            if(str.charAt(i) >= 'A' && str.charAt(i) <= 'Z'){
                stack.push(number[str.charAt(i) - 'A']);
            } else{
                if(!stack.isEmpty()){
                    double first = stack.pop();
                    double second = stack.pop();
                    switch(str.charAt(i)){
                        case '+':
                            result = second + first;
                            stack.push(result);
                            continue;
                        case '-':
                            result = second - first;
                            stack.push(result);
                            continue;
                        case '*':
                            result = second * first;
                            stack.push(result);
                            continue;
                        case '/':
                            result = second / first;
                            stack.push(result);
                            continue;
                    }
                }
            }
        }
        System.out.printf("%.2f", stack.pop());
    }
}
```



## 문자

### 아스키 코드

<img src="https://user-images.githubusercontent.com/101400894/183340942-5f68faa6-8904-4be1-bc2c-5d1a26a605a9.png" alt="image" style="zoom:100%;" />

+ 48 ~ 57
  + 0~9
+ A ~ Z
  + 65 ~ 90
+ a ~ z
  + 97 ~ 122



#### 알파벳이 z를 넘으면 다시 a로 돌아가게 하기

```java
for(char c : arr){
            if(c >='a' && c <='z'){
                if(c>'z') c-=26; //알파벳은 26개라서 26개만큼 빼주면 다시 a에서 시작
            } else if(c >='A' && c<= 'Z'){
                if(c>'Z') c-=26;
            }
            sb.append(Character.toString(c));
        }
```



## 배열의 정렬

### Arrays.sort()

##### (String, int) 오름차순 정렬

```java
Arrays.sort(배열명)
Arrays.sort(arr);
```



##### (String, int) 내림차순 정렬

```java
Arrays.sort(배열명, Collections.reverseOrder());
Arrays.sort(arr, Collections.reverseOrder());
```



#####  int 배열 부분 정렬

```java
Arrays.sort(배열명, 정렬범위 중 첫번째 index, 정렬범위 중 마지막 index)
Arrays.sort(arr, 0, 4);
```



##### String 배열, 문자열 길이 순서 정렬

```java
Arrays.sort(arr, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});

Arrays.sort(arr, (s1, s2) -> s1.length() - s2.length());
```



##### 객체 배열 정렬

```java
public static class Fruit implements Comparable<Fruit> {
    private String name;
    private int price;
    public Fruit(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String toString() {
        return "{name: " + name + ", price: " + price + "}";
    }

    @Override
    public int compareTo(@NotNull Fruit fruit) {
        return this.price - fruit.price;
    }
}
```

