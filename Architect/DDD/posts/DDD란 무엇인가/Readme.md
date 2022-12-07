### DDD란 무엇인가

1. DDD는 단순한/코딩아키텍처 구성 방법이 아니다. 즉 패턴과 설계와 아키텍처를 모조리 합친 그 무언가 개발의 새로운 패러다임이다.
2. DDD는 기존의 설계 방식과 개발 방식을 새로운 관점으로 보게 해준다.

개발을 하면서 발생하는 문제는 여려 가지가 있지만 크게 두 가지로 구분된다.

1. 사용자(고객, 회사 등)에서 제시하는 복잡하고 자주 변화하는 요구사항
2. 성능이나 협업에서 발생하는 기술적인 어려움

DDD는 보통 첫 번째 원인 해결에 초점을 맞춘 방법론이다.
기획자(도메인 전문가)들의 요구사항을 개발 언어로 변환하는 `비효율`을 최대한 `효율`적으로 바꾸려는 통찰이 집약된 방법론이다.

그러나 DDD는 그 필요성이 크지 않은 상황이라면 도입하는 것에 너무 큰 비용이 든다.
학습해야 할 것도 많고, 거쳐야 할 과정도 복잡하기 때문이다.

그러므로 DDD는 복잡한 비즈니스 상황에서 문제를 해결하기 위해 사용할 수 있는 방법론이라고 할 수 있다.

> 당신의 애플리케이션이 완전히 데이터 중심(data-centric)이며 순수한 CRUD 솔루션에 정말 잘 맞아서, 모든 동작이 간단한 데이터베이스 쿼리를 통해 기본적인 생성, 읽기, 갱신, 삭제 등을 수행할 뿐이라면 DDD가 필요하지 않다. 당신의 팀은 그저 예쁜 데이터베이스 테이블 편집기만 있으면 된다. 당신이 단순한 데이터베이스 개발 도구를 사용해 솔루션을 만들 수 있다면, DDD에 회사의 시간과 돈을 낭비하지 말라.

### DDD 적용 과정

DDD가 복잡하다고 말하는 이유는 실제로 도입하려면 다음의 과정들에 전부 익숙해져야 하기 때문이다.

- DDD에서 말하는 **도메인/서브도메인/바운디드 컨텍스트를 구분하고 정의하기.**
- 이를 바탕으로 **Aggregate/Entity/Value Object/Repository 등을 구현하기.**
- 이에 적합한 아키텍처를 결정하고 개발하기.
- 그리고 팀 구성원이 모두 이에 대한 합의를 이루어야 하고, DDD에 대한 지식들이 학습되어 있어야 한다.

DDD는 마냥 쉽게 도입을 결정할 수 있는 부분이 아니다.
아마도 팀에 DDD를 사용하자고 설득하려면 적지 않은 노력이 들 것이다.

### DDD와 개발 방법론의 관계

에릭 에반스의 DDD 저작 이후로 시대가 지나면서 다음과 같은 기술들과 아키텍처가 화두에 오르게 되었다.

> MSA, Hexagonal Architecture, CQRS Patterns, Event-Driven, Event-Sourcing

신기하게도 DDD는 위와 같은 패턴/아키텍처들과 조합되었을 때, 많은 시너지가 나는 방법론이다.
반 버논의 `Implementing DDD`에서는 실제로 위의 방법론들에 대한 대략적인 소개와 DDD에서 활용 가능성에 대해서 소개해 준다.

이 정도라면, 딱히 필요하지 않더라도 한 번은 공부해 볼만하지 않을까 싶다.
아마도 DDD로 넘어갈 생각이 없더라도 DDD를 통해서 얻을 수 있는 인사이트는 무조건 늘어날 것이다.

### DDD = 데이터 중심 설계에서 벗어나는 것

1. DB를 설계한다.

```plaintext
User, Seat, Reservation, Review, Favorite
Reservation은 User와 Seat을 참조한다.
Review는 Reservation을 참조한다.
Favorite은 User와 Seat을 참조한다.
```

2. 이를 토대로 바로 Java 패키지를 구성한다.

```plaintext
com.example.seat-reservation.controller: UserController.java, SeatController.java, ...
com.example.seat-reservation.service: UserService.java, SeatService.java, ...
com.example.seat-reservation.domain: User.java, Seat.java, ...
com.example.seat-reservation.repository: UserRepository.java, SeatRepository.java, ...
com.example.seat-reservation.SeatReservationApplication.java
```

3. Service에서는 CRUD 로직을 감싼 메서드들을 만든다. 아니면 JPA Repository를 바로 사용한다.

```plaintext
user: createUser(), login(), changePassword()
seat: createSeat(), getSeat(), getSeats(), updateSeat(), deleteSeat()
reservation: createReservation(), getReservation(), deleteReservation(), modifyReservation()
```

4. Controller 또는 Service에서 이를 조합하여 API를 제공한다.

이러한 패턴은 굉장히 일반적이고 효율적인 방식이다. 아마 대부분은 이런 식으로 Spring을 학습했을 것이며, 계속 사용하며 익숙해졌을 것이다.
DDD가 지향하는 아키텍처는 위와 크게 다르지 않다. 하지만 우리가 생각하는 이런 생각의 개념들을 완전히 바꾸지 않으면 DDD를 적용할 수 없다.

DDD에서는

- 도메인 / 서브도메인 / 바운디드 콘텍스트를 정의하고 설계도를 그린다.
- 이를 토대로 Entity, Value Object, Aggregate를 도출해낸다.
- 또한 이를 토대로 패키지 구조를 구성한다.
- 어떤 아키텍처를 도입할 것인지를 결정하고 개발한다. (Layered, Hexagonal 등)

### 기존 코드 VS DDD

데이터 중심의 코드에서는 보통 다음의 패턴으로 개발이 많이 이루어진다.

1. 예약을 생성하는 경우

```java
public void createReservation(User user, Seat seat, LocalDateTime startTime, LocalDateTime endTime){
    Reservation reservation = new Reservation();
    reservation.setUserId(user.id);
    reservation.setSeatId(seat.id);
    reservation.setStartTime(startTime);
    reservtiion.setEndTime(endTime);
    this.reservationRepository.save(reservation);
}
```

위의 코드는 getter와 setter를 통해서 비즈니스 로직을 처리하고 있다.
그러나 DDD에서는 위와 같은 코드를 **Anemic Domain Model(무기력증에 걸린 모델)**이라고까지 부르면서 경계한다.
DDD에서는 위와는 다르게 Entity에 직접 영향을 미치는 코드를 도메인 영역이라는 부분으로 감춰버린다.

```java
public void makeReservation(User user, Seat seat, LocalDateTime startTime, LocalDateTime endTime){
  this.reservationCommandService.makeReservation(user, seat, startTime, endTime);
}
```

2. 예약 시작 시간을 변경해야 하는 경우

또 만약에 예약 시작 시간을 변경해야 할 필요가 있다면 기존에는 다음의 방법을 사용했을 것이다.

```java
reservation.setStartTime(start);
```

하지만 DDD에서는 아래와 같은 인터페이스를 두고 시작 시간을 변경하고자 할 것이다.

```java
public interface Reservation {
  public void changeStartTimeTo(LocalDateTime start);
}

reservation.changeStartTimeTo(start);
```

위처럼, DDD를 적용하면 코드에 대한 가독성과 로직의 흐름을 직관적으로 파악하는 데 많은 도움이 된다.
사실 DDD는 객체지향의 끝판왕이라는 생각이 든다. 우리는 예전에 객체지향을 공부할 때 마치 `실제로 존재하는 객체`가 어떤 행위를 하는 것처럼 구성하는 방법론이라고 배웠다.
DDD는 실제로 일어나야 하는 과정을 자세하게 설명해 주는 데에 능한 방법론이다. 도메인 지식이 적은 사람이 코드를 봤을 때, 메서드 이름만으로 무엇을 하는지 바로 알 수 있을 정도가 되도록 하는 것이 DDD 지향적인 코드의 장점이다.

### 도메인과 바운디드 컨텍스트는 무엇인가

DDD는 용어부터 우리가 알고 있는 것과 다르고 익숙해져야 할 것들이 많다. 특히나 바운디드 컨텍스트를 이해하는 것이 DDD를 이해하는 것의 핵심이다.

시작 전에 간단하게 요약해보자면, DDD는 전략적 설계와 전술적 설계로 나뉜다.

- 전략적 설계 : 개념적으로 도메인과 바운디드 컨텍스트를 정의하고 유비쿼터스 용어를 정리하는 부분
- 전술적 설계 : 세부 아키텍처와 코드의 구현과 관련된 부분

조금씩 다르지만 대략적으로 아래의 순서로 전략적 설계가 진행된다고 보면 된다.

1. 도메인/서브도메인 정의하기
2. 바운디드 컨텍스트 나누기
3. 유비쿼터스 언어 결정하기
4. 컨텍스트 맵 그리기

확실한 건 이 부분은 개발적인 부분보다도, 순수하게 비즈니스적인 비중이 더 많기 때문에 개발자만 참여하는 것이 아닌, 개발자와 실무(보통 기획자) 담당의 협업이 필요한 부분이다.

### 코드에서 객체 정의하기

이제 본격적으로 구현 단계다. 구현 요소는 아래와 같이, Entity, Aggregate, Repository, Value Object 총 4가지로 구성된다.

- Value Object (값 객체)
  - 역할
    - 엔티티의 표현을 풍부하게 해주는 `내부 계산`, `설명` 등을 위한 보조 객체 
  - 주의사항 
    - 불변 객체여야 한다. 그래서 setter나 내부 필드를 바꾸는 어떤 행위도 용납되지 않는다. 

```cs
class Person
{
    private PersonId     _id;
    private Name         _name;
    private PhoneNumber  _landline;
    private PhoneNumber  _mobile;
    private EmailAddress _email;
    private Height       _height;
    private CountryCode  _country;

    public Person(...) { ... }
}

static void Main(string[] args)
{
    var dave = new Person(
        id:       new PersonId(30217),
        name:     new Name("Dave", "Ancelovici"),
        landline: PhoneNumber.Parse("023745001"),
        mobile:   PhoneNumber.Parse("0873712503"),
        email:    Email.Parse("dave@learning-ddd.com"),
        height:   Height.FromMetric(180),
        country:  CountryCode.Parse("BG"));
}
```

- Entity

  - Value Object와의 차이점
    - 엔티티의 식별 필드의 값은 엔티티 생애 주기 내내 불변이어야 한다는 점이다.
  - 특성
    - 개별성
      - 다른 데이터들과 구별이 되어야 하는 데이터들의 묶음어어야 한다. 데이터베이스에서 Primary Key를 가지며, 그 키를 이용해서 무엇인가를 해야 한다면 Entity라고 보면 된다. 
    - 변화가능성
      - 내부적으로 상태가 자주 변화한다. 메서드를 통해 바꾸게 된다. 

- Aggregate
- Repository

# 참고한 사이트

[https://appleg1226.tistory.com/40?category=1008265](https://appleg1226.tistory.com/40?category=1008265)
[https://appleg1226.tistory.com/41](https://appleg1226.tistory.com/41)
[https://appleg1226.tistory.com/43?category=1008265](https://appleg1226.tistory.com/43?category=1008265)
[https://appleg1226.tistory.com/44?category=1008265](<(https://appleg1226.tistory.com/44?category=1008265)>)
