# 서론

Spring Boot를 이용해서 어플리케이션을 만들다 보면 외부에서 특정 값들을 주입받아야 하는 경우가 있다. 예를 들면 AWS의 특정 컴포넌트를 사용하기 위한 secret key가 될 수도 있고 외부 API를 사용하기 위한 API key가 될 수도 있다. 이러한 값들을 소스 코드에 하드 코딩한다면 악의적인 의도를 가진 사람이 값을 탈취하여 사용하면서 큰일로 이어질 수 있다.

따라서 이렇게 중요한 값들을 `application.properties` 혹은 `application.yml` 과 같은 외부 설정값을 관리하는 파일에 적어두고 사용하기도 하고 .jar 파일을 실행하기 위한 커맨드에서 직접 값을 넘겨주기도 한다.

### 설정파일 값 가져오기

아래 예시와 같이, application.yml에  설정해준다.

```yml
cloud:
    aws:
        stack.auto: false
        region.static: ap-northeast-2
        credentials.access-key: [access-key 입력]
        credentials.secret-key: [secret-key 입력]
aws:
    s3:
        image:
            bucket: [기본 버킷주소 등록]
```

아래와 같이, 설정 파일들을 불러 올 수 있어야 합니다. 

```java
@Value("${cloud.aws.credentials.access-key}")
private String accessKey;

@Value("${cloud.aws.credentials.secret-key}")
private String secretKey;

@Value("${cloud.aws.region.static}")
private String region;
```

### Application-{profile}.yml 프로파일 전략 

예를 들어, DB 환경설정을 환경에 따라 다르게 하고 싶은 경우가 있을 수 있을 것입니다. 아래처럼 말이죠. 그 때마다 yml 파일을 수정해야 한다면, 

- local 
- Dev
- Production 

```yml

```
spring.config.activate.on-profile: “prod”

@ConfigurationProperties 클래스 작성 
@ConfigurationProperties 테스트 