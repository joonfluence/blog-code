```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Stream의 reduce 이용
Integer sum1 = numbers.stream().reduce(0, Integer::sum);

// IntStream의 sum 이용
int sum2 = numbers.stream().mapToInt(i -> i).sum();
```
