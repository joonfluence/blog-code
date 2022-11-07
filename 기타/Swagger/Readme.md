# 본론

오늘은 SpringBoot 환경에서 Swagger를 적용하는 방법에 관해서 알아보도록 하겠습니다.

### 정의

Swagger란 개발자가 **REST 웹 서비스를 설계, 빌드, 테스트, 문서화**하는 일을 도와주는 오픈 소스 소프트웨어 프레임워크이다.

### 설치방법

```
implementation "io.springfox:springfox-boot-starter:3.0.0"
implementation "io.springfox:springfox-swagger-ui:3.0.0"
```

![Swagger_ui](./swagger-ui.png)

### Swagger 설정 Bean 등록하기

SwaggerConfig.java 파일을 추가하고 이를 Spring Bean으로 등록해준다. 참고로 @Configuration은 설정파일을 만들기 위한 애노테이션 or Bean을 등록하기 위한 애노테이션이다.

```java
@Configuration
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }
}
```

- **Docket** : Swagger 설정의 핵심이 되는 Bean.
- **useDefaultResponseMessages** : Swagger 에서 제공해주는 기본 응답 코드 (200, 401, 403, 404). true 로 설정하면 기본 응답 코드를 노출.
- **apis** : api 스펙이 작성되어 있는 패키지 (Controller) 를 지정.
- **paths** : apis 에 있는 API 중 특정 path 를 선택.

### application.properties 설정

Spring boot 2.6버전 이후인 경우, spring.mvc.pathmatch.matching-strategy 값이 ant_apth_matcher에서 path_pattern_parser로 변경되면서 몇몇 라이브러리(swagger포함)에 오류가 발생하므로, application.properties에 아래와 같이 설정을 추가해주면 오류가 발생하지 않습니다.

```yml
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```

### Controller 설정

```java
@RestController
public class BasicController {

    @GetMapping("/api/hello1")
    public String hello1() {
        return "hello";
    }

    @GetMapping("/api/hello2")
    public String hello2(@RequestParam String param) {
        return param;
    }
}
```

### 확인 방법

3.x : http://localhost:8080/swagger-ui/index.html
2.x : http://localhost:8080/swagger-ui.html

# 참고한 사이트

[https://castleone.tistory.com/2](https://castleone.tistory.com/2)
