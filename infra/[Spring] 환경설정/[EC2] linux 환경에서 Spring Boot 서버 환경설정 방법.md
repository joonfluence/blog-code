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

### 적용 확인

java -version

# 참고한 사이트

