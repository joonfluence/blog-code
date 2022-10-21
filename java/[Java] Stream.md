# 서론

오늘은 Java의 Stream에 관해서 알아보도록 하겠습니다.

### 정의

데이터 처리연산을 지원하도록 소스에서 추출된 연속된 요소를 말함. 

### 스트림을 쓰는 이유

스트림 API는 데이터를 추상화하여 다루므로, 다양한 방식으로 저장된 데이터를 읽고 쓰기 위한 공통된 방법을 제공합니다.
따라서 스트림 API를 이용하면 **배열**이나 **컬렉션**뿐만 아니라 **파일**에 저장된 데이터도 모두 같은 방법으로 다룰 수 있게 됩니다.

### 특징

1. 스트림은 외부 반복을 통해 작업하는 컬렉션과는 달리 내부 반복(internal iteration)을 통해 작업을 수행합니다.
2. 스트림은 재사용이 가능한 컬렉션과는 달리 단 한 번만 사용할 수 있습니다.
3. 스트림은 원본 데이터를 변경하지 않습니다.
4. 스트림의 연산은 필터-맵(filter-map) 기반의 API를 사용하여 지연(lazy) 연산을 통해 성능을 최적화합니다.
5. 스트림은 parallelStream() 메소드를 통한 손쉬운 병렬 처리를 지원합니다.

### 자주 쓰이는 함수 정리

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Stream의 reduce 이용
Integer sum1 = numbers.stream().reduce(0, Integer::sum);

// IntStream의 sum 이용
int sum2 = numbers.stream().mapToInt(i -> i).sum();
```

# 참고한 사이트

[http://tcpschool.com/java/java_stream_concept](http://tcpschool.com/java/java_stream_concept)
