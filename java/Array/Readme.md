# 서론

오늘은 Java에서 ArrayList에 제공되는 다양한 메서드들에 관해 알아보는 시간을 갖겠습니다.

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

### 교집합

arrayList에서 arrayList2와 중복된 값만 남김.

```java
arrayList.retainlAll(arrayList2);
```

### 차집합

arrayList에서 arrayList2와 같은 리스트 값을 모두 제거하여, 반환된다.

```java
arrayList.removeAll(arrayList2);
```

### 부분집합

arrayList의 부분집합이 반환된다.

```java
arrayList.containsAll(arrayList2);
```

### 합집합

인자를 포함한 결괏값이 반환된다.

```java
arrayList.addAll(arrayList2);
```

### 일치 여부 조회

```java
Arrays.equals(array1, array2);
```

# 참고한 사이트

[https://ddolcat.tistory.com/794](https://ddolcat.tistory.com/794)
[https://codechacha.com/ko/java-collections-arraylist-initialization/](https://codechacha.com/ko/java-collections-arraylist-initialization/)
