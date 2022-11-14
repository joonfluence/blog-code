### JPA Auditing

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity{

    // Entity가 생성되어 저장될 때 시간이 자동 저장됩니다.
    @CreatedDate
    private LocalDateTime createdDate;

    // 조회한 Entity 값을 변경할 때 시간이 자동 저장됩니다.
    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

```java
@EnableJpaAuditing
@Configuration
public class JpaAuditingConfiguration {

}
```

### MappedSuperclass

```java
@MappedSuperclass
public abstract class BaseEntity {
  @Id @GeneratedValue
  private Long id;
  private String name;
}
```

```java
public class Member extends BaseEntity {
  // ID 상속
  // NAME 상속
  private String email;
  ...
}
```

```java
public class Seller extends BaseEntity {
  // ID 상속
  // NAME 상속
  private String shopName;
  ...
}
```
