# 서론

오늘은 CI(Continous Integration) / CD(Continuous Delivery)에 관해서 알아보도록 하겠습니다.

# 본론

### CI란

CI란 버그 수정이나 새로 만드는 기능들이 메인 레포지토리에 지속적으로 빌드되고 테스트가 되어서 머지되는 것을 말합니다.

**CI에서 중요한 포인트**

1. 코드 변경사항을 주기적으로 빈번하게 머지해야 한다.
2. 통합을 위한 단계(머지, 빌드, 테스트)의 자동화.

지속적으로 머지 충돌을 막을 수 있다. 머지되는 코드들은 자동으로 빌드되고 테스트되기 때문에 코드의 문제점을 빠르게 해결할 수 있습니다.

### CD란

Continous Delivery 혹은 Continous Deployment를 말합니다. CI를 통해, **주기적으로 머지된 코드의 변경사항들이 자동으로 빌드되고 테스트되었다면 배포 준비 과정을 거쳐, 수동적으로 배포하는 단계**를 거칩니다. 이를 전자라고 합니다. **자동화된 경우**를 후자라고 말합니다.

### CI/CD를 해야 하는 이유

- 개발 생산성을 높이기 위함

CI/CD를 쓰면 하나의 동작만으로 배포가 될 수 있습니다. 따라서, 배포를 위해 생각전환을 할 필요가 없습니다. 만약 CI/CD를 하지 않으면, 배포를 위한 설정을 매번 해줘야 합니다. 그 과정에서 에러가 발생될 수도 있고, 그로인해 개발자의 개발 생산성이 저하되게 됩니다.

- Git과 연동해서 사용하기 좋습니다.

git에서 제공하는 모듈이 있어서, git push 한 번만으로도 배포가 될 수 있도록 만들 수 있습니다. 예를 들어, git과 연동해서 CI/CD를 구성하면 deploy 브랜치에 커밋을 했을 때, release로 바로 반영되게 됩니다. Git-hub에선 git-hub action을 쓰고, bit-bucket에선 pipeline을 사용합니다.

# 참고한 사이트

[https://www.youtube.com/watch?v=0Emq5FypiMM&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9](https://www.youtube.com/watch?v=0Emq5FypiMM&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9)