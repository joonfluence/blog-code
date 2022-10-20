# 본론

### Gradle 프로젝트 생성하기

초기에 프로젝트를 생성하면 다음과 같은 파일이 생성됩니다.

```plaintext
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   └── resources
    └── test
        ├── java
        └── resources
```

각 파일의 역할에 관해서 알아보도록 하겠습니다.

- build.gradle
  - 정의
    - Project 오브젝트(객체)로 Project 인터페이스를 구현하는 구현체를 말함. Project 오브젝트는 Project 단위에서 필요한 작업을 수행하기 위해 모든 메서드와 프로퍼티를 모아놓은 슈퍼 객체.
  - 구성
    - plugins
      - 특정 작업을 하기 위해 모아놓은 Task들의 묶음
    - repositories
      - 오픈 소스 라이브러리를 호스팅하는 저장소
    - dependencies
      - 프로젝트 구성을 위해 받아와야 할 라이브러리를 정의해놓은 공간

```groovy
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

// 둘 다 많이 사용함
repositories {
    mavenCentral()
    jcenter()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

test {
    useJUnitPlatform()
}
```

- gradle-wrapper.properties
  - Gradle 작업을 선언된 버전으로 실행하는 스크립트
- gradlew
  - 내장 그레이들을 사용하기 위한 스크립트로, Mac/Linux용이다
- gradlew.bat
  - 윈도우용 스크립트
- settings.gradle
  - 멀티 모듈 간 상호 참조를 관리하고 하나의 디렉토리로 다중 프로젝트를 관리함으로써 효과적인 유지보수 체계를 제공한다.

# 참고한 사이트

[https://kotlinworld.com/321](https://kotlinworld.com/321)
[https://waspro.tistory.com/648](https://waspro.tistory.com/648)
