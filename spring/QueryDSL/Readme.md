# 본론

오늘은 QueryDSL의 정의와 사용하는 이유 및 배경에 관해서 알아보도록 하겠습니다.

### 동적 쿼리란

대표적으로 QueryDSL을 사용하는 경우가 2가지 있습니다. `복잡한 쿼리를 작성하는 경우`와 `동적 쿼리를 작성하는 경우`입니다.

상황에 따라 다른 문법의 SQL을 적용하는 것을 말합니다. 예를 들어, 게시글(Post) 검색 기능을 구현한다고 가정해봅시다. 게시글을 조회하는 기준과 조회하는 쿼리 문은 아래와 같을 것입니다.
중복해서 조회할 경우를 포함하면 총 7개의 쿼리문이 필요하므로, 중복적이고 비효율적인 상황이 발생됩니다.

- 제목 (title)
- 생성일자 (createdAt)
- 작성자 (author)

```sql
SELECT * FROM Post WHERE title = #{title};
SELECT * FROM Post WHERE createdAt = #{createdAt};
SELECT * FROM Post WHERE author = #{author};
SELECT * FROM Post WHERE title = #{title} AND createdAt = #{createdAt};
SELECT * FROM Post WHERE title = #{title} AND author = #{author};
SELECT * FROM Post WHERE createdAt = #{createdAt} AND author = #{author};
SELECT * FROM Post WHERE title = #{title} AND createdAt = #{createdAt} AND author = #{author};
```

하지만 이런 상황을 QueryDSL을 활용하면, 쉽게 해결할 수 있습니다.

### QueryDSL이란

QueryDSL은 쿼리를 문자가 아니라 자바 코드로 작성할 수 있게 돕는 도구입니다. 동적 쿼리 문제를 깔끔하게 해결할 뿐만 아니라, 문법 오류도 컴파일 시점에 모두 잡아줍니다. 자바 코드로 작성하짐나 SQL, JPQL과 문법이 거의 같기 때문에 쉽게 학습할 수 있고, 또 쉽게 복잡한 쿼리도 작성할 수 있습니다.

### 설정 방법

```groovy
// build.gradle

buildscript {
    ext {
        queryDslVersion = "5.0.0"
    }
}

plugins {
    id 'com.ewerk.gradle.plugins.querydsl' version "1.0.10"
    ...
}

//querydsl에서 사용할 경로를 선언
def querydslDir = "$buildDir/generated/querydsl"

dependencies {
  implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
  implementation "com.querydsl:querydsl-apt:${queryDslVersion}"
}

//querydsl 설정을 추가합니다. JPA 사용 여부와 사용할 경로
querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}

//build시 사용할 sourceSet
sourceSets {
    main.java.srcDir querydslDir
}
//querydsl이 compileClassPath를 상속하도록 설정
configurations {
    querydsl.extendsFrom compileClasspath
}
//querydsl 컴파일시 사용할 옵션을 설정
compileQuerydsl {
    options.annotationProcessorPath = configurations.querydsl
}
```

스프링 빈에도 등록해줍니다.

```java
package com.tutti.backend.config;

import com.querydsl.jpa.impl.JPAQueryFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Configuration
public class QueryDslConfig {
    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}
```

# 참고한 사이트

[QueryDSL을 사용하는 이유](https://www.inflearn.com/course/Querydsl-%EC%8B%A4%EC%A0%84)
[QueryDSL이란](https://tecoble.techcourse.co.kr/post/2021-08-08-basic-querydsl/)
[[Querydsl] 다이나믹 쿼리 사용하기](https://jojoldu.tistory.com/394)
