### 배열의 길이 구하기 : length

```java
int[] arr = new int[5];
int tmp = arr.length; // 5
```

### 배열의 복사 : System.arraycopy

```java
System.arraycopy(num, 0, newNum, 0, num.length);
```

배열 num의 내용을 배열 newNum으로, 배열 num의 첫 번째 요소(num[0])부터 시작해서 num.length개의 데이터를 newNum의 첫번째 요소(newNum[0])에 복사한다.

### 이차원 배열 만드는 방법

```java
int[][] score = new int[4][3];
int[][] arr = {{1, 2, 3}, {4, 5, 6}};
```

