# 서론

오늘은 트랜잭션에 대해서 알아보도록 하겠습니다.

# 본론

### 트랜잭션이란?

데이터베이스(이하 DB)에서 더 이상 나눌 수 없는 최소의 실행 단위를 말합니다. 또 DB는 하나의 트랜잭션을 수행할 때 온전히 그 명령이 실행되는 것 보장하며, 실행 도중 오류가 발생하면 해당 쿼리를 실행하기 이전 상태로 되돌림으로써 데이터의 일관성을 보장합니다.

### 트랜잭션의 실행과정

DB에서는 명령이 끝날 때까지 수행 내역을 로그에 보관합니다. DB에서 반영된 내용을 재반영하기 위한 Redo Log와 수행에 실패해 이전의 상태로 되돌리는 Undo Log를 이용해, 트랜잭션이 수행될 수 있도록 합니다.

### 트랜잭션의 성질

1. 원자성 (Atomicity)
  - 트랜잭션은 DB 작업의 논리적인 작업 단위로, 트랜잭션은 DB에 모두 반영되거나 아니면 전혀 반영되지 않아야 한다는 것입니다.
2. 독립성 (Isolation)
  - 어떤 하나의 트랜잭션이라도, 다른 트랜잭션의 연산에 끼어들 수 없다는 점을 말합니다.
3. 지속성 (Durability)
  - 트랜잭션이 완료됐을 때, 결과는 영구적으로 반영되어야 합니다.
4. 일관성 (Consistency)
  - 트랜잭션의 작업 처리 결과가 항상 일관성 있어야 한다는 것이다.

### 예시

설명이 어려우니 예를 들어, 설명해보겠습니다. 사용자(A)가 다른 사용자(B)에게 송금해야 하는 상황을 생각해봅시다. A의 통장에서 보낼 금액만큼 차감되고 B의 통장에는 A가 보낸만큼 금액이 추가되어야 합니다. 작업 도중 하나만 성공하고 나머지가 실패한다면, 문제가 발생될 것입니다. 일련의 과정이 모두 성공해야, 작업이 성공될 수 있습니다.

하나로 묶여 있는 동작이 성공적으로 수행되면, 이를 데이터베이스에 반영해야 합니다. 이는 DB가 일관성 있는 상태이며, 하나의 트랜잭션이 끝났다는 의미로 `트랜잭션 커밋`이라고 합니다. 이 연산을 사용하면 수행했던 트랜잭션이 로그에 저장되며, 후에 **Rollback 연산의 새로운 기준점**이 됩니다. 반대로 하나의 트랜잭션 처리가 비정상적으로 종료되어 원자성이 꺠진 경우, 트랜잭션을 처음부터 다시 시작하거나, 트랜잭션의 부분적으로만 연산된 결과를 취소하는 경우를 `롤백`이라고 합니다. 

### 스프링에서의 트랜잭션

1. 인터페이스

스프링은 트랜잭션 추상화 인터페이스인 PlatformTransactionManager을 제공하여, 다양한 DataSource에 맞게 트랜잭션을 관리할 수 있도록 돕습니다. getTransaction, rollback, commit으로 구성됩니다. getTransaction 메서드를 통해, 파라미터로 전달되는 TransactionDefinition에 따라 트랜잭션을 시작합니다. 트랜잭션을 문제 없이 마치면 commit을, 문제가 발생하면 rollback 메서드를 실행합니다. 

```java
public interface PlatformTransactionManager extends TransactionManager {
  TransactionStatus getTransaction(@Nullable TransactionDefinition definition);
  void commit(TransactionStatus status);
  void rollback(TransactionStatus status);
}
```

2. 구현체

스프링엔 대표적으로 DataSourceTransactionManager, JpaTransactionManager, JtaTransactionManager가 있습니다. 아래와 같이, 코드 상으로 직접 구현하는 방식이 있습니다. 

```java
public void method(){
  PlayformTransactionManager transactionManager = new DataSourceTransactionManager(datasource);

  // 트랜잭션 시작
  TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());

  try {
    update(user); // 비즈니스 로직 
    transactionManager.commit(status);
  } catch(Exception e){
    transactionManager.rollback(status);
    throw e;
  }
}
```

그 외에도 AOP를 이용한 선언적 트랜잭션을 사용할 수 있습니다. 선언적 트랜잭션은 `tx 네임스페이스를 사용하는 방안`과 `어노테이션을 기반으로 설정하는 방안`이 있습니다. 전자는 Bean 설정 파일에서 트랜잭션 매니저를 등록하고 속성과 대상을 정의해 트랜잭션을 적용하겠다고 명시하는 것입니다. 이렇게 적용하면 코드에는 영향을 주지 않고 *일괄적으로 트랜잭션을 적용하고 변경할 수 있다는 장점*이 있습니다. 

```
<bean id="transactionManager" class="org.springframework.jdbc.dataSource.DataSourceTransactionManager">
  <property name="dataSource"></property>
</bean>
```

```
<tx:advice id="txAdvice" transaction-manager="transactionManager">
  <tx:attributes>
    <tx:method name="*" />
  </tx:attributes>
</tx:advice>
```

```
<aop:config>
  <aop:pintcut id="txPointcut" expresstion="execution(* *..MemberDao.*(..))" />
  <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut" />
</aop:config>
```

트랜잭션 어노테이션은 메서드, 클래스, 인터페이스 등에 적용할 수 있습니다. 클래스 상단에 적용된 어노테이션은 클래스의 모든 메서드에 적용되며, 하위 메서드에 어노테이션이 중첩될 경우에는 클래스 메서드, 클래스, 인터페이스 메서드, 인터페이스 순으로 우선순위를 갖고 적용됩니다. 

어노테이션이 적용된 메서드는 메서드 시작부터 트랜잭션이 시작되고, 메서드를 성공적으로 끝마치면 트랜잭션 커밋, 도중에 문제가 발생하면 롤백하는 과정이 진행됩니다. 어노테이션은 데이터베이스에 여러 번 접근하면서 하나의 작업을 수행하는 서비스 계층 메서드에 붙이는 것이 통상적입니다. 

```java
@Service
@Transactional
public class CrewService {
  private final CrewRepository crews;

  public CrewService(final CrewRepository crews){
    this.crews = crews;
  }

  @Transactional
  public sendMoney(){
    Crew sally = crews.findByName("샐리");
    if(sally.money < 10000){
      throw new IllegalArgumentException("잔액이 부족합니다.");
    }
    sally.moeny -= 10000;
    Crew bada = crews.findByName("바다");
    bada.money += 10000;
  }
}
```


# 참고한 사이트

[[10분 테코톡] 🐤 샐리의 트랜잭션](https://www.youtube.com/watch?v=aX9c7z9l_u8)
[[10분 테코톡] 🙊 에이든의 트랜잭션 메커니즘](https://www.youtube.com/watch?v=ImvYNlF_saE)
[트랜잭션이란?](https://mommoo.tistory.com/62)