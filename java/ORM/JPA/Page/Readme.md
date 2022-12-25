### Paging 구현

- 관련 패키지

페이징과 정렬 파라미터

```groovy
org.springframework.data.Sort // 정렬 기능
org.springframework.data.Pageable // 페이징 기능, 내부에 Sort 포함
org.springframework.data // 내부에 Sort와 Pageable 클래스가 존재한다. 즉, 모든 DB의 페이징이 공통화 되있음을 의미한다.
```

특별한 반환타입

```groovy
org.springframework.data.domain.Page // 페이지 + Total Count 쿼리 포함
org.springframework.data.domain.Slice // 페이지만 확인, 내부적으로 limit + 1조회
List // 추가 count 쿼리 없이 결과만 반환(페이징 기능 없음)
```

- JPA 레포지토리 작성하기

```java
Page<Member> findByAge(int age, Pageable pageable);
```

파라미터(PageRequest, Pageble의 구현체) 전달해주기

```java
PageRequest pageRequest = PageRequest.of(0, 3, Sort.by(Sort.Direction.DESC, "username"));
```

# 참고한 사이트

[https://ojt90902.tistory.com/716](https://ojt90902.tistory.com/716)
