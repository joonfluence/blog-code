### Server(Spring) & Client(Jquery)

---

- [문제]
  - 415 UnsupportedType 문제 발생됨
- [예상원인]
  - Content type 'application/x-www-form-urlencoded;charset=UTF-8' not supported
- [해결방안]
  - 전송할 때, JSON 형태로 전달할 것. - JSON 형태로 어떻게 전송할 수 있을까? - 전송했음. - 서버에서 Json 데이터를 처리 못함. - @PostMapping(path = "", consumes = APPLICATION_JSON_VALUE) 추가해줬음.

---

- [문제]
  - **[Junit 5 - No ParameterResolver registered for parameter](https://stackoverflow.com/questions/51867650/junit-5-no-parameterresolver-registered-for-parameter)**
- [해결방안]
  - ```java
      @Autowired CustomerCorpMapper customerCorpMapper;
    ```

---

- [문제]
  - TestCode 상 MyBatis Mapper가 제대로 동작하지 않는 문제
- [해결방법]
  ```java
    import org.mybatis.spring.boot.test.autoconfigure.MybatisTest;
    @MybatisTest
  ```
- [참고한사이트]
  - https://github.com/mybatis/spring-boot-starter/issues/353

---

- [문제]
  - Failed to load ApplicationContext
- [해결방법] [https://kjh95.tistory.com/343](https://kjh95.tistory.com/343)
- @SecheduleID Prameter Error
  - The proxy server received an invalid response from an upstream server.The proxy server could not handle the request.
