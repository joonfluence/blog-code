# 서론

오늘은 자바 프로그램을 만들 때, 많은 도움을 받을 수 있는 `lombok`에 관해서 알아보도록 하겠습니다.

# 본론

### 설치방법

```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

    testCompileOnly 'org.projectlombok:lombok:1.18.12' // 테스트 의존성 추가
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.12' // 테스트 의존성 추가
}
```

### Lombok은 무엇이고 왜 쓰는 것일까?

아래와 같은 클래스가 있을 때, 변수는 `private` 선언하고 `getter/setter`를 사용합니다.

```java
public class User {
    private String id;
    private String password;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

그런데 멤버변수가 20개, 30개로 늘어나게 되면 작성해야 할 getter/setter 코드가 증가하게 됩니다. 또한, 필드가 하나 수정되면 getter/setter 코드들을 전부 수정해줘야 한다는 단점이 있습니다. 이러한 단점을 해결하기 위해서 나온 것이 `lombok`입니다. `Lombok`은 자바 라이브러리이고, @Data, @Getter, @Setter, @Builder 등등의 다양한 어노테이션을 제공합니다. 이번글에서는 어노테이션 의미를 간단하게 정리해보겠습니다.

### 활용할 메서드

@Getter : getter 메서드를 생성한다.
@Setter : setter 메서드를 생성한다.
@Slf4g : 로그를 출력하기 위한 메서드를 제공한다.

# 결론

이상으로 자바 라이브러리 중 하나인 `lombok`에 대해서 알아보았습니다.

# 참고한 사이트

[https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/3%EC%A3%BC%EC%B0%A8/Lombok%20%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0.md](https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/3%EC%A3%BC%EC%B0%A8/Lombok%20%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0.md)
