# 본론

오늘은 Gradle로 멀티 모듈 프로젝트를 구성하는 방법에 관해서 알아보도록 하겠습니다. 또 2편에서는 추가로 JPA와 QueryDSL, MapStruct를 설정하고 실제 프로젝트 의존성을 설정해주도록 하겠습니다.

## 서비스 분리

- 비대해지는 프로그램을 좀 더 효율적으로 관리하기 위하여 다양한 방법이 있겠지만 여기서는 MSA와 모듈로 분리하는 방법에 대해 간략히 살펴보겠습니다.

### MSA로 분리하는 이유

- 첫 번째와 두 번째 예는 서로 다른 상황이지만 오리지널 프로젝트를 기반으로 하여 서비스를 확장해 나가야 한다는 공통점이 있습니다. 확장을 위한 선택지로 마이크로 서비스(Micro Service Architecture : MSA)를 생각해볼 수 있습니다. 첫 번째 예시의 경우 서로 다른 성격의 서비스가 하나의 프로젝트 안에 공존하고 있어 아래 그림처럼 독립적으로 동작하는 마이크로 서비스로 변경하는 것을 고려해 볼 수 있습니다.

![MSA](https://daddyprogrammer.org/wp-content/uploads/2020/07/multimodule-diagram-696x930.png)

### 모듈로 분리하는 이유

![멀티모듈](https://daddyprogrammer.org/wp-content/uploads/2020/07/multimodule-diagram-696x930.png)

#### 멀티 모듈을 쓰는 이유

- 단일 소스를 기반으로 서비스가 모듈화되므로 공통으로 사용하는 소스코드의 유지 보수에 유리하다

### 방법

- 새로운 프로젝트 생성하기
- 의존성 확인하기
- 공통 모듈에서 서브 모듈 연결시키기

### 추가로 해야할 작업

# 참고한 사이트

[https://daddyprogrammer.org/post/13156/spring-boot-change-multi-module/](https://daddyprogrammer.org/post/13156/spring-boot-change-multi-module/)
[https://github.com/joonfluence/gradle-multi-module](https://github.com/joonfluence/gradle-multi-module)
[https://techblog.woowahan.com/2637/](https://techblog.woowahan.com/2637/)
[https://jojoldu.tistory.com/m/123](https://jojoldu.tistory.com/m/123)
[https://spring.io/guides/gs/multi-module/](https://spring.io/guides/gs/multi-module/)
[https://skryvets.com/blog/2017/08/21/install-gradle-and-configure-gradle-home-on-a-mac/](https://skryvets.com/blog/2017/08/21/install-gradle-and-configure-gradle-home-on-a-mac/)
