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

## 기본개념

CI/CD에서 가장 중요한 것은 `자동화`입니다. 오늘은 Github Action을 위한 CI/CD 자동화 방법에 관해서 알아보도록 하겠습니다.

### Github Actions 정리하기

1. Events

main 브랜치로 머지하거나, **커밋을 푸쉬**하거나, 이슈를 누군가가 여는 등. 깃헙에서 일어날 수 있는 일에 대하여, 여러가지 이벤트들을 미리 지정해줄 수 있습니다.

2. Workflows

이벤트(예를 들면, 커밋 푸쉬)가 실행될 때, 수행되어야 할 일(Job)들의 목록을 말합니다. Workflow는 하나 혹은 다수의 job으로 구성됩니다. Job에는 Unit Test, E2E Test 등이 포함될 수 있습니다.

3. Jobs

어떤 순서대로 실행되어야 하는지, 실행되어야 하는 명령어들을 추가해줄 수 있습니다. 예를 들어, Job에는 유닛 테스트를 실행하거나 E2E 테스트를 실행하는 등의 명령이 포함될 수 있습니다.

4. Actions

재사용할 수 있는 Actions가 있습니다. 원하는 명령어를 실행할 수 있고, 이미 오픈소스로 공개된 actions를 불러와 사용할 수도 있습니다. 자동화하고자 하는 Job을 스크립트 형태로 저장할 수도 있습니다.

5. Runners

각각의 Job은 병렬적으로 실행되는데, 이를 실행하는 요소가 Runner 입니다. Runner는 Virtual Machine 혹은 Docker Container가 해당될 수 있습니다.


# 참고한 사이트

[https://www.youtube.com/watch?v=0Emq5FypiMM&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9](https://www.youtube.com/watch?v=0Emq5FypiMM&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9)
