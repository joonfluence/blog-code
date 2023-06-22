# 서론

오늘은 Spring Boot 환경에서 JPA Auditing을 활용하여 생성/수정시간 자동화하는 방법에 대해서 알아보겠습니다.

# 본론

BaseTimeEntity란 클래스를 만들어 생성과 수정 시간이 필요한 모든 요소의 상위 클래스로 지정하여, Entity들의 createdDate, modifiedDate를 자동으로 관리하겠습니다.

### JPA Auditing

Entity 클래스의 상단에 @MappedSuperclass를 추가해줍니다. 이를 추가함으로써, JPA Entity 클래스들이 BaseTimeEntity을 상속할 경우 필드들(createdDateTime, modifiedDateTime)도 칼럼으로 인식하도록 만듭니다. 또 @EntityListeners(AuditingEntityListner.class)를 활용하여 BaseTimeEntity 클래스에 Auditing 기능을 포함해줍니다.

```
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity{

    // Entity가 생성되어 저장될 때 시간이 자동 저장됩니다.
    @CreatedDate
    private LocalDateTime createdDateTime;

    // 조회한 Entity 값을 변경할 때 시간이 자동 저장됩니다.
    @LastModifiedDate
    private LocalDateTime modifiedDateTime;
}
```

마지막으로 SpringApplication 실행 메서드 상에 @EnableJpaAuditing 어노테이션을 추가해줍니다.

```
@EnableJpaAuditing // JPA Auditing 활성화
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### 활용

```
public class Member extends BaseTimeEntity {
  // ID 상속
  // NAME 상속
  private String email;
  ...
}
```

```
public class Post extends BaseEntity {
  // ID 상속
  // NAME 상속
  private String contentName;
  ...
}
```

### 테스트해보기

```
@Test
public void BaseTimeEntity_등록() {
    // given
    LocalDateTime now = LocalDateTime.of(2022, 6, 22, 0, 0, 0);
    postsRepository.save(Posts.builder().title("title").content("content").author("author").build());

    // when
    List<Posts> postsList = postsRepository.findAll();
    Posts posts = postsList.get(0);

    System.out.println("createDate >> " + posts.getCreatedDate() + " // modifiedDate >> " + posts.getModifiedDate());

    // then
    assertThat(posts.getCreatedDate()).isAfter(now);
    assertThat(posts.getModifiedDate()).isAfter(now);
}
```

위 코드를 실행해보면, 정상적으로 동작하는 것을 확인 할 수 있습니다.
