# 서론

람다란 무엇일까요? 람다식의 도입으로 인해, 자바는 객체지향언어인 동시에 함수형 언어가 되었습니다.

```java
List<String> list = Arrays.asList("abc", "aaa", "bbb");
Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```

### 메서드 참조

람다식이 하나의 메서드만 호출할 경우, `메서드 참조`라는 방법으로 람다식을 간략하게 만들 수 있습니다.

```java
Function<String, Integer> f = (String s) -> Integer.parseInt(s);
Function<String, Integer> f = Integer::parseInt;
```
