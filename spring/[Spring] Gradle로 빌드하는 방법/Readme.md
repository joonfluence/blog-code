# 본론

### 빌드 명령어 입력

Gradle의 경우

```shell
# build 폴더를 생성한다
./gradlew build

# build 폴더를 삭제하고 다시 빌드
./gradlew clean build
```

Maven의 경우

```shell
# target 폴더 안에 .jar가 생성됨
./mvnw package
```

### build/libs 폴더에 생성된 jar 파일을 확인

일반적으로 서버에 배포할 때는 hello-spring-0.0.1-SNAPSHOT.jar 파일을 서버에 올려서 실행하면 배포 완료.

### jar 파일 실행 방법

```shell
java -jar hello-spring-0.0.1-SNAPSHOT.jar
```

### jar 파일 실행 종료 방법

```shell
taskkill /IM java.exe /F
```

# 참고한 사이트

[https://mosei.tistory.com/entry/Spring-Boot-Gradle-%EB%A1%9C-%EB%B9%8C%EB%93%9C-%ED%95%98%EB%8A%94-%EB%B2%95](https://mosei.tistory.com/entry/Spring-Boot-Gradle-%EB%A1%9C-%EB%B9%8C%EB%93%9C-%ED%95%98%EB%8A%94-%EB%B2%95)
