# 서론

# 본론

### shell

```shell
sudo apt update
sudo apt install default-jre
sudo apt install default-jdk
```

### 환경변수 적용

```shell
vi /etc/bash.bashrc
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
source /etc/bash.bashrc
```

### 빌드방법

```shell
./gradlew build
```

### 빌드된 파일 실행 방법

```shell
./gradlew run
```

### 적용 확인

java -version

### 서버 파일 권한 부여

권한 부여 없이, 파일을 전송하면 permission denied 에러가 뜰 수 있다. 따라서 폴더에 접근할 수 있도록 권한을 풀어줘야 한다.

```shell
chmod -R 777 [폴더명]
```

폴더 권한을 다음 예제처럼 쓰면 모든사용자가 쓰기, 읽기, 실행하도록 바꿀수 있다.
위에서 설명한 바와 같이 여기서 777은 rwx(user 권한) rwx(group 권한) rwx(other 권한) 으로 r: 읽기권한, w: 쓰기권한 x: 실행권한 이라 부른다.
만약 예제 명령어처럼 chmod -R 777 filename일 때, 777이라고 넣으면 user, group, other에게 모든권한을 전부 준다는 의미가 된다는 것이다.

### 빌드된 파일 EC2로 전송하기

```shell
scp -i [PemKey경로] [빌드파일경로] <username>@<public-ip or DNS>:/pathwhere/you/needto/copy
```

# 참고한 사이트

[https://cloudkatha.com/how-to-deploy-spring-boot-application-on-aws-ec2/](https://cloudkatha.com/how-to-deploy-spring-boot-application-on-aws-ec2/)
[https://88240.tistory.com/13](https://88240.tistory.com/13)
