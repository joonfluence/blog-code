# 서론

# 본론

### 도커란

도커는 어플리케이션 패키징 툴 입니다. 플랫폼과 상관없이 구동 가능하도록 어플리케이션 코드, 환경설정, 의존성 등 앱을 구동하기 위한 정보들을 포괄하여 플랫폼과 상관없이 구동 가능하도록 돕습니다.
또 컨테이너 기반의 가상화 플랫폼입니다. 부두에서 컨테이너를 옮기고 관리하는 직업인 docker에서 따 온 이름에 걸맞게 컨테이너를 잘 다룰 수 있게 도와 주는 도구라고 할 수 있지요.

### 가상화란

가상화란 하나의 하드웨어를 여러 개의 가상 머신으로 분할해 효율적으로 사용할 수 있는 기술을 말합니다. 분할된 가상 머신들은 각각 독립적인 환경으로 구동됩니다. 이 때, 베이스가 되는 기존의 환경을 `Host OS`, 가상 머신으로 분할된 각각의 환경을 `Guest OS`라고 부릅니다.

### 컨테이너란

하이퍼바이저와 달리 컨테이너는 베이스 환경의 OS를 공유하면서 필요한 프로세스만 격리하는 방식으로, 커널을 공유하기 때문에 호스트 OS의 기능을 모두 사용할 수 있습니다. 그렇기 때문에 컨테이너 위에서는 호스트 OS와 다른 OS를 구동할 수 없습니다.

### VM과의 차이

Host OS를 갖고 있느냐 없느냐를 기준으로 구분 지을 수 있습니다. 컨테이너는 포함하지 않고 Host OS를 공유합니다.

### Mac 설치 방법

```shell
brew install --cask docker
```

cask 옵션을 주게 되면 Docker Desktop on Mac을 설치하게 되고 docker-compose, docker-machine을 같이 설치해줘 한결 편리하게 사용할 수 있습니다. 설치가 완료되면 데스크탑에서 docker를 실행해봅시다.

### Ubuntu 설치 방법

```shell
sudo apt-get update
```

```shell
sudo apt-get install ca-certificates curl gnupg lsb-release
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Docker의 Official GPG Key 를 등록합니다.

```shell
 echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

stable repository 를 등록해줍니다.

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

위의 명령어를 입력하면 자동으로 최신 버전이 설치 됩니다.

### 버전 확인

```shell
docker --version
```

# 마무리

[https://ssyoni.tistory.com/m/22](https://ssyoni.tistory.com/m/22)
[https://velog.io/@jaryeonge/Docker-Mac%EC%97%90-Homebrew%EB%A1%9C-docker-%EC%84%A4%EC%B9%98](https://velog.io/@jaryeonge/Docker-Mac%EC%97%90-Homebrew%EB%A1%9C-docker-%EC%84%A4%EC%B9%98)
[https://velog.io/@markany/%EB%8F%84%EC%BB%A4%EC%97%90-%EB%8C%80%ED%95%9C-%EC%96%B4%EB%96%A4-%EA%B2%83-1.-%EB%8F%84%EC%BB%A4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80](https://velog.io/@markany/%EB%8F%84%EC%BB%A4%EC%97%90-%EB%8C%80%ED%95%9C-%EC%96%B4%EB%96%A4-%EA%B2%83-1.-%EB%8F%84%EC%BB%A4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
[https://shanepark.tistory.com/237](https://shanepark.tistory.com/237)
