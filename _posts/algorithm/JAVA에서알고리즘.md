## JAVA에서

#### 배열 초기화는 Arrays.fill 함수를 쓰자 {#배열-초기화는-arraysfill-함수를-쓰자}

보통 배열을 초기화 할때, for 구문을 2중 3중으로 돌려서 초기화 한다. Java에서는 C언어의 memset와 같이 어떤 배열을 한번에 초기화 하는 함수를 제공한다.

```java
int memo[][] = new int[101][101];

for (int i = 0; i < memo.length; i++) {    \
    Arrays.fill(this.memo[i], - 1);  // 2중 for문이 아니라 1중 for로 초기화 가능
}
```

주의점: Arrays.fill은 배열의 하나의 depth만 일괄 초기화가 가능하다. 따라서 위 예제와 같이 2차원 배열일때, 2차원을 한번에 초기화는 불가능하다. 그렇더라도, 2중 for문에 비하면 성능이 좋고, 1차원 배열의 경우 한번에 초기화 해주고 있다.

#### Java char 초기화와 String 초기화를 구별하자 {#java-char-초기화와-string-초기화를-구별하자}

Java에서 char 초기화와 String 초기화

보통 코드 작성시 String으로 대부분의 동작이 가능하지만, char로 바꾸어서 하나의 문자만 비교해야 하는 경우가 가끔 있다.

따라서 char 와 String의 초기화 방법을 잘 알아 두어야 한다.

```java
// String
String str = null; // (O) null 초기화 가능
String str = "";   // (O) 빈문자열 초기화 가능

// char
char c = null;     // (X) null 초기화 이렇게 하면 안됨
char c = '\u0000'; // (O) null 초기화는, unicode을 사용해야함, 다행이 null은 0000임, 싱글 쿼테이션 써야함
char c = '';       // (X) 빈문자열 초기화 불가능
char c = ' ';      // (O) 공백 하나 띄우고 초기화는 가능
```

#### Java 어떤 Type의 Max 값을 설정할때는, 해당 Type의 MAX\_VALUE을 사용하자 {#java-어떤-type의-max-값을-설정할때는-해당-type의-maxvalue을-사용하자}

Java에서, Integer나 Float, Double 의 Max 값을 설정해야 하는경우, 해당 Type에서 제공하는 MAX\_VALUE을 사용하면 간단하게 처리 가능하다.

```
int int_INFINITY = Integer.MAX_VALUE;
float float_INFINITY = Float.MAX_VALUE;
double double_INFINITY = Double.MAX_VALUE;
```

#### Java에서 문자열을 분리할때, StringTokenizer을 사용하자 {#java에서-문자열을-분리할때-stringtokenizer을-사용하자}

Java에서 문자열을 분리할때, 간단하게`split`함수를 사용 할 수 있다. 사용하기 편리하다 하지만 속도는`StringTokenizer`가 더 빠르다. 따라서, 속도가 중요할 경우에는`StringToknizer`을 사용하는 것이 좋다.

```java
// 일반적으로 사용
StringTokenizer st = new StringTokenizer("this is a test");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}

// BufferedReader 와 같이 사용할때
StringTokenizer token = new StringTokenizer(br.readLine());

int val1 = Integer.parseInt(token.nextToken());
int val2 = Integer.parseInt(token.nextToken());
```

split 사용법도 알아두면 좋다. 가장 많이 사용하는 공백자르기 2개는 꼭 기억하자.

```java
// "\\s" 는 공백(space)로 자르기 이다.
String[] result = "this is a test".split("\\s");

// "\\s+" 는 공백(space)가 가변적으로 (스페이스가 하나 이상)인 경우도 정상적으로 잘라주는 표현이다.
for (int x=0; x<result.length; x++)
    System.out.println(result[x]);
```

속도비교 - 참고:[https://github.com/hughperkins/jfastparser](https://github.com/hughperkins/jfastparser)  
Scanner: 10642 ms

```
split: 715 ms
StringTokenizer: 544ms
JFastParser: 290ms
```



