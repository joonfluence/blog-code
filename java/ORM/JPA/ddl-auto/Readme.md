# 서론

오늘은 JPA ddl-auto 설정 방법에 관해서 알아보겠습니다.

# 본론

### ddl-auto의 정의

JPA에서는 기본적으로 Entity에 테이블을 매핑하면 쿼리를 사용하지 않고 값을 가져올 수 있습니다.

```yaml
jpa:
  hibernate:
    ddl-auto: create #create-drop, update, validate, none
```

- create : 기존테이블 삭제 후 다시 생성 (DROP + CREATE)
- create-drop : create와 같으나 종료시점에 테이블 Drop
- update : 변경분만 반영
- validate : 엔티티와 테이블이 정상 매핑되었는지만 확인
- none : 아무것도 안함

### 주의할 점

- 운영 장비에서는 절대 create, create-drop, update 사용하면 안된다.
- 개발 초기 단계는 create 또는 update.
- 테스트 서버는 update 또는 validate.
- 스테이징과 운영 서버는 validate 또는 none.

하지만 로컬 환경을 제외한 나머지 서버에서는 최대한 직접 쿼리를 날려서 적용하는 것이 좋음.

# 참고한 사이트

[https://haservi.github.io/posts/spring/hibernate-ddl-auto/](https://haservi.github.io/posts/spring/hibernate-ddl-auto/)
