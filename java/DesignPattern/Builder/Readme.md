# 본론

### 정의

생성과 관련된 디자인 패턴으로, 동일한 프로세스를 거쳐 다양한 구성의 인스턴스를 만드는 방법 중 하나이다.
빌더 패턴은 복잡한 객체를 생성하는 클래스와 표현하는 클래스를 분리하여, 동일한 절차에서도 서로 다른 표현을 생성하는 방법을 제공한다.
생성해야 하는 객체가 Optional한 속성을 많이 가질 때 더 좋다.

### 사용하는 이유

객체를 생성하는 디자인 패턴 중 가장 효과적이기 떄문임.

- 점층적 생성자 패턴

  - 단점

    - Optional한 인자에 따라 새로운 생성자를 만들거나, Null 값으로 채워야하는 문제가 발생된다.

      ```java
      /**
      * 기본 생성자 (필수)
      */
      public TourPlan() {
      }

      /**
      * 일반적인 여행 계획 생성자
      *
      * @param title 여행 제목
      * @param startDate 출발 일
      * @param nights n박
      * @param days m일
      * @param whereToStay 머물 장소
      * @param plans n일차 할 일
      */
      public TourPlan(String title, LocalDate startDate, int nights, int days,
          String whereToStay, List<DetailPlan> plans) {
          this.title = title;
          this.nights = nights;
          this.days = days;
          this.startDate = startDate;
          this.whereToStay = whereToStay;
          this.plans = plans;
      }

      /**
      * 당일치기 여행 계획 생성자
      *
      * @param title 여행 제목
      * @param startDate 출발 일
      * @param plans 1일차 할 일
      */
      public TourPlan(String title, LocalDate startDate, List<DetailPlan> plans) {
          this.title = title;
          this.startDate = startDate;
          this.plans = plans;
      }
      ```

    - Lombok의 @AllArgsConstructor 에너테이션을 활용하면 코드가 길어지는 문제는 해결할 수 있지만, 여전히 많은 경우 타입과 순서로 발생할 수 있는 에러 가능성이 존재한다.

      ```java
      // 순서를 파악이 어렵고, 가독성이 떨어진다.
      new TourPlan("여행 계획", LocalDate.of(2021,12, 24), 3, 4, "호텔",
          Collections.singletonList(new DetailPlan(1, "체크인")));

      // 생성자를 만들지 않고 당일치기 객체를 생성하면 불필요한 Null을 채워야한다.
      new TourPlan("여행 계획", LocalDate.of(2021,12, 24), null, null, null,
          Collections.singletonList(new DetailPlan(1, "놀고 돌아오기")));
      ```

- 자바 빈 패턴

  - (앞선 패턴에 비했을 때의) 장점
    - 가독성이 나아진다.
    - 순서에 상관 없어, 에러 발생 가능성이 줄어든다.
  - 단점
    - 함수 호출이 인자만큼 이루어지고, 객체 호출 한번에 생성할 수 없다.
    - immutable 객체를 생성할 수 없다.
      - 쓰레드간 공유 가능한 객체 일관성(consistency)이 일시적으로 꺠질 수 있다.

  ```java
  TourPlan tourPlan = new TourPlan();
  tourPlan.setTitle("칸쿤 여행");
  tourPlan.setNights(2);
  tourPlan.setDays(3);
  tourPlan.setStartDate(LocalDate.of(2021, 12, 24));
  tourPlan.setWhereToStay("리조트");
  tourPlan.addPlan(1, "체크인 이후 짐풀기");
  tourPlan.addPlan(1, "저녁 식사");
  tourPlan.addPlan(2, "조식 부페에서 식사");
  tourPlan.addPlan(2, "해변가 산책");
  tourPlan.addPlan(2, "점심은 수영장 근처 음식점에서 먹기");
  tourPlan.addPlan(2, "리조트 수영장에서 놀기");
  tourPlan.addPlan(2, "저녁은 BBQ 식당에서 스테이크");
  tourPlan.addPlan(3, "조식 부페에서 식사");
  tourPlan.addPlan(3, "체크아웃");
  ```

- 빌더 패턴

  - 장점
    - 필요한 객체를 직접 생성하는 대신, 먼저 필수 인자들을 생성자에 전부 전달하여 빌더 객체를 만든다.
    - 그리고 선택 인자는 가독성이 좋은 코드로 인자를 넘길 수 있다.
    - setter가 없으므로 객체 일관성을 유지하여 불변 객체로 생성할 수 있다.

  ```java
  public interface TourPlanBuilder {
    TourPlanBuilder nightsAndDays(int nights, int days);
    TourPlanBuilder title(String title);
    TourPlanBuilder startDate(LocalDate localDate);
    TourPlanBuilder whereToStay(String whereToStay);
    TourPlanBuilder addPlan(int day, String plan);
    TourPlan getPlan();
  }
  ```

  이를 구현하는 ConcreteBuilder는 다음과 같다.

  ```java
  public class DefaultTourBuilder implements TourPlanBuilder {
    private String title;
    private int nights;
    private int days;
    private LocalDate startDate;
    private String whereToStay;
    private List<DetailPlan> plans;

    @Override
    public TourPlanBuilder nightsAndDays(int nights, int days) {
        this.nights = nights;
        this.days = days;
        return this;
    }

    @Override
    public TourPlanBuilder title(String title) {
        this.title = title;
        return this;
    }

    @Override
    public TourPlanBuilder startDate(LocalDate startDate) {
        this.startDate = startDate;
        return this;
    }

    @Override
    public TourPlanBuilder whereToStay(String whereToStay) {
        this.whereToStay = whereToStay;
        return this;
    }

    @Override
    public TourPlanBuilder addPlan(int day, String plan) {
        if (this.plans == null) {
            this.plans = new ArrayList<>();
        }

        this.plans.add(new DetailPlan(day, plan));
        return this;
    }

    @Override
    public TourPlan getPlan() {
        return new TourPlan(title, startDate, days, nights, whereToStay, plans);
    }
  }
  ```

  이제 TourPlan 객체를 생성하는 코드를 살펴보자.

  ```java
  return tourPlanBuilder.title("칸쿤 여행")
        .nightsAndDays(2, 3)
        .startDate(LocalDate.of(2020, 12, 9))
        .whereToStay("리조트")
        .addPlan(0, "체크인하고 짐 풀기")
        .addPlan(0, "저녁 식사")
        .getPlan();
  ```

  Director를 적용하면 클라이언트 코드가 더 짧아질 수 있다.

  ```java
  public class TourDirector {
    private TourPlanBuilder tourPlanBuilder;

    public TourDirector(TourPlanBuilder tourPlanBuilder) {
        this.tourPlanBuilder = tourPlanBuilder;
    }

    public TourPlan cancunTrip() {
        return tourPlanBuilder.title("칸쿤 여행")
                .nightsAndDays(2, 3)
                .startDate(LocalDate.of(2020, 12, 9))
                .whereToStay("리조트")
                .addPlan(0, "체크인하고 짐 풀기")
                .addPlan(0, "저녁 식사")
                .getPlan();
    }

    public TourPlan longBeachTrip() {
        return tourPlanBuilder.title("롱비치")
                .startDate(LocalDate.of(2021, 7, 15))
                .getPlan();
    }
  }
  ```

# 참고한 사이트

[https://dev-youngjun.tistory.com/197#:~:text=%EB%B9%8C%EB%8D%94%20%ED%8C%A8%ED%84%B4%EC%9D%80%20%EB%B3%B5%EC%9E%A1%ED%95%9C%20%EA%B0%9D%EC%B2%B4,%ED%95%98%EB%8A%94%20%EB%B0%A9%EB%B2%95%EC%9D%84%20%EC%A0%9C%EA%B3%B5%ED%95%9C%EB%8B%A4.](https://dev-youngjun.tistory.com/197#:~:text=%EB%B9%8C%EB%8D%94%20%ED%8C%A8%ED%84%B4%EC%9D%80%20%EB%B3%B5%EC%9E%A1%ED%95%9C%20%EA%B0%9D%EC%B2%B4,%ED%95%98%EB%8A%94%20%EB%B0%A9%EB%B2%95%EC%9D%84%20%EC%A0%9C%EA%B3%B5%ED%95%9C%EB%8B%A4.)
[https://hangjastar.tistory.com/246](https://hangjastar.tistory.com/246)
