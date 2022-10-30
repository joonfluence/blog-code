# 서론

오늘은 Java에서 ArrayList에 제공되는 다양한 메서드들에 관해 알아보는 시간을 갖겠습니다.

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
