# 본론

## 기본개념

### Github Action을 위한 CI/CD 자동화 방법

전에는 Jenkins + Buildkite

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

### 실제 적용하기

1. 스프링 빌드 자동화

```shell
# .github/workflows/workflow.yml
name: Java CI with Gradle

on:
  push:
    branches:
      - 'develop'

  pull_request:
    branches:
      - 'develop'

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        arguments: build
```

github의 actions에 들어가서 처리 할 것

2. merge develop branch to master branch

```shell

```

3. Deploy via S3 using AWS Code Deploy

```shell
ame: logging-system

on:
  workflow_dispatch:

env:
  S3_BUCKET_NAME: s3-unanimous
  PROJECT_NAME: unanimous

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    # zip 파일을 압축한다.
      - name: Make zip file
        run: zip -r ./$GITHUB_SHA.zip .
        shell: bash

    # AWS credentials 사용한다.
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.ImageCodeDeployAccessKey }}
          aws-secret-access-key: ${{ secrets.ImageCodeDeploySecretKey }}
          aws-region: ${{ secrets.ImageCodeDeployRegion }}

    # jar 파일을 S3에 업로드한다.
      - name: Upload to S3
        run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.zip s3://$S3_BUCKET_NAME/$PROJECT_NAME/$GITHUB_SHA.zip

    # Code Deploy로 자동 배포한다.
      - name: Code Deploy
        run: aws deploy create-deployment --application-name logging-system-deploy --deployment-config-name CodeDeployDefault.AllAtOnce --deployment-group-name develop --s3-location bucket=$S3_BUCKET_NAME,bundleType=zip,key=$PROJECT_NAME/$GITHUB_SHA.zip
        working-directory: ./unanimous
```

4. application property 설정하기 

```shell
name: make application.properties
run: |
  cd ./src/main/resources
  touch ./application.properties
  echo "${{ secrets.PROPERTIES }}" > ./application.properties
shell: bash
```

github secrets에 추가해줄 것. 

### 배포된 서버 자동으로 실행하기

```shell
# run_new_was.sh

#!/bin/bash

CURRENT_PORT=$(cat /home/ubuntu/service_url.inc | grep -Po '[0-9]+' | tail -1)
TARGET_PORT=0

echo "> Current port of running WAS is ${CURRENT_PORT}."

if [ ${CURRENT_PORT} -eq 8081 ]; then
  TARGET_PORT=8082
elif [ ${CURRENT_PORT} -eq 8082 ]; then
  TARGET_PORT=8081
else
  echo "> No WAS is connected to nginx"
fi

TARGET_PID=$(lsof -Fp -i TCP:${TARGET_PORT} | grep -Po 'p[0-9]+' | grep -Po '[0-9]+')

if [ ! -z ${TARGET_PID} ]; then
  echo "> Kill WAS running at ${TARGET_PORT}."
  sudo kill ${TARGET_PID}
fi
JAR_NAME=$(ls -tr /home/ubuntu/[프로젝트명]/build/libs/game-0.0.1-SNAPSHOT.jar | tail -n 1)
echo "> JAR Name: $JAR_NAME"
chmod +x $JAR_NAME
nohup java -jar -Dserver.port=${TARGET_PORT} $JAR_NAME > /home/ubuntu/nohup.out 2>&1 &
echo "> Now new WAS runs at ${TARGET_PORT}."
exit 0
```

# 참고사이트

[https://www.youtube.com/watch?v=iLqGzEkusIw&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9](https://www.youtube.com/watch?v=iLqGzEkusIw&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9)
[https://www.youtube.com/watch?v=R8_veQiYBjI&ab_channel=TechWorldwithNana](https://www.youtube.com/watch?v=R8_veQiYBjI&ab_channel=TechWorldwithNana)
[https://velog.io/@wlrhkd49/Github-Actions%EB%A1%9C-AWS-EC2-%EC%84%9C%EB%B2%84%EC%97%90-%EB%B0%B0%ED%8F%AC-%EC%9E%90%EB%8F%99%ED%99%94](https://velog.io/@wlrhkd49/Github-Actions%EB%A1%9C-AWS-EC2-%EC%84%9C%EB%B2%84%EC%97%90-%EB%B0%B0%ED%8F%AC-%EC%9E%90%EB%8F%99%ED%99%94)
[https://jojoldu.tistory.com/281](https://jojoldu.tistory.com/281)
[https://jojoldu.tistory.com/282](https://jojoldu.tistory.com/282)
[https://jojoldu.tistory.com/283](https://jojoldu.tistory.com/283)
