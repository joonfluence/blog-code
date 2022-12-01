## 스프링 빈

일반적으로 기존의 Java Programming 에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용했었습니다. 하지만 Spring에서는 직접 new를 이용하여 생성한 객체가 아니라, **Spring이 직접 관리하는 자바 객체**를 사용합니다. 이렇게 Spring에 의하여 생성되고 관리되는 자바 객체를 `Bean`이라고 합니다. Spring Framework에서는 `Spring Bean`을 얻기 위하여, ApplicationContext.getBean()와 같은 메소드를 사용하여 Spring에서 직접 자바 객체를 얻어서 사용합니다. 이처럼 *객체 생성의 주체를 위임하는 것*을 **제어의 역전(Inversion Of Control, 줄여서 IOC)**라고 합니다.

## 스프링 컨테이너 (ApplicationContext)

ApplicationContext란 BeanFactory 인터페이스의 하위 인터페이스이다. 즉, ApplicationContext는 BeanFactory에 부가기능을 추가한 것을 말한다. `BeanFactory`는 스프링 컨테이너의 최상위 인터페이스로, 스프링 빈을 관리하고 조회하는 역할을 한다. ApplicationContext는 BeanFactory + 부가 기능(국제화 기능, 환경 변수 관련 처리, 애플리케이션 이벤트, 리소스 조회)을 가진다.

## 스프링 빈 등록 방식

### Component Scan

### Java 코드로 등록

# 참고한 사이트

[https://steady-coding.tistory.com/594](https://steady-coding.tistory.com/594)
[https://melonicedlatte.com/2021/07/11/232800.html](https://melonicedlatte.com/2021/07/11/232800.html)
