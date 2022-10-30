### 정의

MapStruct란 Entity와 Dto간의 매핑을 지원하는 라이브러리를 말한다.

### 사용하는 이유

데이터 조회 API를 개발하는 경우를 생각해보자. 컨트롤러를 통해서 들어온 요청이 여러 비즈니스 로직과 데이터 접근 로직을 거치면서 여러 계층 간의 데이터 변경을 한 후에 최종적으로 응답을 반환한다.

DTO(Data Transfer Object)와 Entity 간의 변환을 예로 들 수 있다. 보통 빌더를 사용했다면 아래와 같은 형태로 객체 간의 매핑을 했얼 것이다.

```java
ArticleEntity toEntity(Article article) {
  return ArticleEntity.builder()
    .id(article.getId())
    .articleTypeCode(article.getType().getCode())
    .title(article.getTitle())
    .author(article.getWriter())
    .build();
}
```

# 참고한 사이트

[Mapstruct-in-springboot](https://madplay.github.io/post/mapstruct-in-springboot)
