# 본론

### 실제 적용하기

1. 스프링 빌드 자동화

```yaml
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

```yaml
- name: Merge branch
  # You may pin to the exact commit or the version.
  # uses: devmasx/merge-branch@854d3ac71ed1e9deb668e0074781b81fdd6e771f
  uses: devmasx/merge-branch@1.4.0
  with:
    type: now
    # The branch name or hash to merge. default GITHUB_SHA
    target_branch: master
    # Github token
    github_token: ${{ secrets.TOKEN }}
```

3. Deploy via S3 using AWS Code Deploy

- IAM 유저 Code Deploy 권한 부여
  - `AmazonEC2RoleforAWS-CodeDeploy`
- 역할을 EC2 서비스에 등록
  - 인스턴스 설정
    - IAM 역할 연결/바꾸기
- EC2 접속 후 CodeDeploy 에이전트 설치
- CodeDeploy를 위한 권한 생성
  - 개체 선택
  - 권한 선택
  - 태그 등록
  - 이름 등록 및 최종 확인
- CodeDeploy 생성
  - 애플리케이션 생성
  - 배포 그룹 생성
- Github-Action S3, CodeDeploy 연동

  - Code Deploys는 Github에 올라간 파일들을 EC2에 전달하는 기능

    ```yaml
    name: Deploy String boot to Amazon EC2

    on:
      push:
        branches:
          - master

    env:
      S3_BUCKET_NAME: [버킷명]
      PROJECT_NAME: [프로젝트명]
      APPICATION_NAME: [CodeDeploy 앱 명]

    jobs:
      deploy:
        name: DEPLOY
        runs-on: ubuntu-latest

        steps:
          - name: Checkout
            uses: actions/checkout@v2

          - name: Set up JDK 1.8
            uses: actions/setup-java@v1
            with:
              java-version: 1.8

          - name: Grant execute permission for gradlew
            run: chmod +x gradlew
            shell: bash

          - name: Build with Gradle
            run: ./gradlew build -x test
            shell: bash

          - name: Make zip file
            run: zip -qq -r ./$GITHUB_SHA.zip .
            shell: bash
        # AWS credentials 사용한다.
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: ${{ secrets.AWS_REGION }}

        # jar 파일을 S3에 업로드한다.
          - name: Upload to S3
            run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.zip s3://$S3_BUCKET_NAME/$PROJECT_NAME/$GITHUB_SHA.zip

        # Code Deploy로 자동 배포한다.
          - name: Code Deploy
            run: aws deploy create-deployment --application-name $APPICATION_NAME --deployment-config-name CodeDeployDefault.AllAtOnce --deployment-group-name develop --s3-location bucket=$S3_BUCKET_NAME,bundleType=zip,key=$PROJECT_NAME/$GITHUB_SHA.zip
            working-directory: ./unanimous
    ```

  - 빌드된 Jar를 통해 스프링부트 자동 실행

    - 빌드 스크립트 작성

      ```shell
      #!/bin/bash

      REPOSITORY = /home/ec2-user/app/step2
      PROJECT_NAME = springboot2-webservice

      echo "> Build 파일 복사"
      cp $REPOSITORY/zip/build/libs/*.jar $REPOSITORY/

      echo "> 현재 구동 중인 애플리케이션 pid 확인"

      # 실행 중이면 종료하기 위해서 현재 수행 중인 프로세스id를 찾습니다.
      # springboot2-webservice으로 된 다른 프로그램들이 있을 수 있어 springboot2-webservice된 jar 프로세스를 찾은 뒤 id를 찾습니다(awk '{print $1}').
      CURRENT_PID = $(pgrep -fl springboot2-webservice | grep java | awk '{print $1}')

      echo "현재 구동 중인 애플리케이션 pid: $CURRENT_PID"

      if [ -z "$CURRENT_PID" ]; then
        ehco "> 현재 구동 중인 애플리케이션이 없으므로 종료하지 않습니다."
      else
        echo "> kill -15 $CURRENT_PID"
        kill -15 $CURRENT_PID
        sleep 5
      fi

      echo "> 새 애플리케이션 배포"

      JAR_NAME = $(ls -tr $REPOSITORY/*.jar | tail -n 1)

      echo "> JAR name: JAR_NAME"
      echo "> $JAR_NAME에 실행 권한 추가"
      chmod +x $JAR_NAME

      echo "> $JAR_NAME 실행"

      nohup java -jar \
          -Dspring.config.location=classpath:/application.properties,classpath:/application-real.properties,/home/ec2-user/app/application-oauth.properties,/home/ec2-user/app/application-real-db.properties \
          -Dspring.profiles.active=real \
          $JAR_NAME > $REPOSITORY/nohup.out 2>&1 & # nohup실행 시 CodeDeploy는 무한 대기 합니다. 이 이슈를 해결하기 위해 nohub.out파일을 표준 입출력용으로 별도로 사용합니다. 이렇게 하지 않으면 nohup.out파일이 생기지 않고, CodeDeploy 로그에 표준 입출력이 출력됩니다. nohub이 끝나기 전까지 CodeDeploy도 끝나지 않으니 꼭 이렇게 해야합니다.
      ```

    - .circleci/config.yml 파일 수정

      - 현재까지는 프로젝트의 모든 파일을 zip파일로 만드는데, 실제로 필요한 파일들은 Jar, appspec, yml, 배포 scipt들입니다.

        ```yaml
        ...(생략)...
        jobs:
          build:

          .....(생략).....

              - run:
                  name: before_deploy
                  command: |
                    tar cvzf springboot2-webservice.tgz scripts/*.sh appspec.yml build/libs/*.jar

              - persist_to_workspace:
                  root: .
                  paths: .

          deploy:
            executor: my-executor

            steps:
              - attach_workspace:
                  at: .

              - aws-s3/copy:
                  from: springboot2-webservice.tgz
                  to: 's3://hwany-springboot-build'
                  aws-region: AWS_DEFAULT_REGION

              - aws-code-deploy/deploy-bundle:
                  application-name: hwany-springboot2-webservice
                  deployment-group: hwany-springboot2-webservice-group
                  deployment-config: CodeDeployDefault.AllAtOnce
                  bundle-bucket:  hwany-springboot-build
                  bundle-key: springboot2-webservice # 확장자 제거해야 함
                  bundle-type: tgz

          workflows:
          .....(생략).....
        ```

      - appspec.yml 파일 수정

        ```yaml
        version: 0.0 # CodeDeploy 버전
        os: linux
        files:
          - source: / # CodeDeploy에서 전달해 준 파일 중 destination으로 이동시킬 대상을 루트로 지정(전체파일)
            destination: /home/ec2-user/app/step2/zip/ # source에서 지정된 파일을 받을 위치, 이후 jar를 실행하는 등은 destination에서 옮긴 파일들로 진행
            overwrite: yes

        permissions: # CodeDeploy에서 EC2서버로 넘겨준 파일들을 모두 ec2-user권한을 갖도록 합니다.
          - object: /
            pattern: "**"
            owner: ec2-user
            group: ec2-user

        hooks: # CodeDeploy배포 단계에서 실행할 명령어를 지정합니다.
          ApplicationStart: # deploy.sh를 ec2-user권한으로 실행합니다.
            - location: scripts/deploy.sh
              timeout: 60 # 스크립트 실행 60초 이상 수행되면 실패가 됩니다.
              runas: ec2-user
        ```

# 참고사이트

[https://velog.io/@hwany/AWS-EC2-CodeDeploy-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0](https://velog.io/@hwany/AWS-EC2-CodeDeploy-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)
[https://jojoldu.tistory.com/281](https://jojoldu.tistory.com/281)
[https://jojoldu.tistory.com/282](https://jojoldu.tistory.com/282)
[https://jojoldu.tistory.com/283](https://jojoldu.tistory.com/283)
