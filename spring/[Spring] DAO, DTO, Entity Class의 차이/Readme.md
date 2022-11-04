# 서론

### DAO(Data Access Object)란

- 실제로 DB에 접근하는 객체이다.
  - **Persistence Layer(DB에 data를 CRUD하는 계층)**이다.
- Service와 DB를 연결하는 고리의 역할을 한다.
- SQL을 사용(개발자가 직접 코딩)하여 DB에 접근한 후 적절한 CRUD API를 제공한다.
  - JPA는 대부분 기본적인 CRUD method를 제공한다.
  - `extends JpaRepository<User, Long>`

```java
// JPA 사용 시
public interface QuestionRepository extends CrudRepository<Question, Long> {}
```

### DTO(Data Transfer Object)란

- 계층 간 데이터 교환을 위한 객체(Java Bean)이다.
  - DB에서 데이터를 얻어 Service나 Controller 등으로부터 보낼 때 사용하는 객체를 말한다.
  - 즉, DB의 데이터가 Presentation Logic Tier로 넘어오게 될 땐 DTO의 모습으로 바뀌어서 오고 가고 있는 것이다.
  - 로직을 갖고 있지 않는 **순수한 데이터 객체**이며, getter/setter 메서드 만을 갖는다.
  - 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO 클래스에는 setter가 없다.

# 참고한 사이트

[https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html](https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html)
