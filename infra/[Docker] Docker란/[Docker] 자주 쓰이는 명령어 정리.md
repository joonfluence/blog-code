### Dockerfile

Dockerfile은 DockerImage를 생성하기 위한 스크립트(설정파일)입니다. 여러가지 명령어를 토대로 Dockerfile을 작성한 후 빌드하면, Docker는 Dockerfile에 나열된 명령문을 차례대로 수행하며 DockerImage를 생성해준다.
Dockerfile을 읽을 줄 안다는 것은 해당 이미지가 어떻게 구성되어 있는지 알 수 있다는 의미이다.

### Dockerfile의 장점

- 이미지가 어떻게 만들어졌는지를 기록한다.

이미지는 어플리케이션(이하 앱)을 담고 있는데, 앱 설치 과정은 어떠한지 중간에 어떤 과정을 수정해야 하는지 등을 알아야 한다.

- 배포에 용이하다.

어떠한 이미지를 배포할 때, 몇 기가씩이나 되는 이미지 파일 자체를 배포하기보다는 그 이미지를 만들 수 있는 스크립트인 Dockerfile만을 배포하면 된다.

- 컨테이너(이미지)가 특정 행동을 수행하도록 한다.

컨테이너 환경에서 앱을 개발하다 보면, 특정 행동을 취하도록 하는 컨테이너를 만들어야 할 때가 있다.

### Dockerfile 작성 및 명령어

Dockerfile을 작성할 땐 실제 파일의 이름을 `Dockerfile`로 해야 합니다.
ubuntu에 아파치 서버를 설치하는 Dockerfile을 작성해보도록 하겠습니다.

```shell
mkdir apache-dockerfile && cd apache-dockerfile
vi Dockerfile
```

```shell
FROM adoptopenjdk/openjdk11
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

- FROM : 베이스 이미지
- MAINTAINER : 이미지를 생성한 개발자의 정보 (1.13.0 이후 사용 X)
- LABEL : 이미지에 메타데이터를 추가 (key-value 형태)
- RUN : 새로운 레이어에서 명령어를 실행하고 새로운 이미지를 생성
- WORKDIR : 작업 디렉토리를 지정해준다.
- EXPOSE : Dockerfile의 빌드로 생성된 이미지에서 열어줄 포트를 의미한다.
- USER : 이미지를 어떤 계정에서 실행할지 지정한다.
- COPY / ADD : build 명령 중간에 호스트의 파일 또는 폴더를 이미지에 가져온다.
- ENV : 환경 변수 값을 지정할 수 있다.
- CMD / ENTRYPOINT : 컨테이너를 생성, 실행할 때 실행할 명령어를 말한다.
  - CMD는 docker run 실행 시, 추가적인 명령어에 따라 설정한 명령어를 수정하고자 할 때 사용된다.
  - ENTRYPOINT는 docker run 실행 시, 추가적인 명령어의 존재 여부와 상관 없이 무조건 실행되는 명령을 말한다.

### 각종 명령어들

📚 도커의 기본 명령어들
❗️ 명령 입력 시 permission 관련 오류가 뜨는 환경에서는 각 명령어 앞에 sudo를 붙여주세요.

⭐️ 도커 버전 확인

```shell
docker -v
```

도커 이미지 다운만 받기

```shell
docker pull {이미지명}:{태그}
# 예: docker pull python:3
```

태그는 필수가 아닙니다.

⭐️ 컴퓨터 내 도커 이미지들 보기

```shell
docker images
```

이미지로 컨테이너 생성하기

```shell
docker create {옵션} {이미지명}:{태그}
# 예: docker create -it python
```

만들어진 컨테이너 시작하기 (이미지에 CMD로 지정해놓은 작업 시키기)

```shell
docker start {컨테이너 id 또는 이름}
```

컨테이너로 들어가기 (컨테이너 내 CLI 이용하기)

```shell
docker attach {컨테이너 id 또는 이름}
```

⭐️ 이미지를 다운받아(없을 시에만) 바로 컨테이너 실행하여 진입하기

```shell
docker run {이미지명}:{태그}
# 예: docker -it run python:3
```

pull, create, start, attach 를 한꺼번에 실행하는 것과 같습니다.

**옵션설명**
-d 데몬으로 실행(뒤에서 - 안 보이는 곳(백그라운드)에서 알아서 돌라고 하기)
-it 컨테이너로 들어갔을 때 bash로 CLI 입출력을 사용할 수 있도록 해 줍니다.
--name {이름} 컨테이너 이름 지정
-p {호스트의 포트 번호}:{컨테이너의 포트 번호} 호스트와 컨테이너의 포트를 연결합니다.
--rm 컨테이너가 종료되면{내부에서 돌아가는 작업이 끝나면} 컨테이너를 제거합니다.
-v {호스트의 디렉토리}:{컨테이너의 디렉토리} 호스트와 컨테이너의 디렉토리를 연결합니다.

동작중인 컨테이너 재시작

```shell
docker restart {컨테이너 id 또는 이름}
```

도커 컨테이너의 내부 쉘에서 빠져나오기 (컨테이너를 종료)

```shell
exit
```

또는 Ctrl + D

도커 컨테이너의 내부 쉘에서 빠져나오기 (컨테이너를 종료하지 않음)

Ctrl + P, Q

⭐️ (동작중인) 컨테이너들 보기

```shell
docker ps
```

동작중이 아닌 것을 포함한 모든 컨테이너를 보려면 -a 옵션을 뒤에 붙입니다.

컨테이너 삭제

```shell
docker rm {컨테이너 id 또는 이름}

# ⭐️ 모든 컨테이너 삭제
docker rm `docker ps -a -q`
```

이미지 삭제

```shell
docker rmi {옵션} {이미지 id}
```

컨테이너가 있을 시 강제삭제: -f 옵션 사용

⭐️ 모든 컨테이너와 이미지 등 도커 요소 중지 및 삭제

```shell
# 모든 컨테이너 중지
docker stop $(docker ps -aq)

# 사용되지 않는 모든 도커 요소(컨테이너, 이미지, 네트워크, 볼륨 등) 삭제
docker system prune -a
```

```shell
# 아래를 복붙하여 함께 실행하면 편리합니다.
docker stop $(docker ps -aq)
docker system prune -a
```

확인 질문에 y로 답하고 마무리합니다.

⭐️ 도커파일로 이미지 생성

```shell
# Dockerfile 파일이 있는 디렉토리 기준.  마지막의 . 이 상대주소
docker build -t {이미지명} .
docker build -f Dockerfile -t [이미지 이름:이미지 버전] [Dockerfile의 경로]
# M1의 경우
docker build -f Dockerfile -t [이미지 이름:이미지 버전] [Dockerfile의 경로] --platform linux/amd64
```

⭐️ 도커 컴포즈 실행

```shell
# docker-compose 파일이 있는 디렉토리 기준
docker-compose up
```

Docker Compose는 여러개의 도커 어플리케이션 컨테이너들을 정의하고 실행 할 수 있게 도와주는 툴 입니다. YAML 파일을 사용해 어플리케이션의 서비스를 설정하고 하나의 커맨드만으로 여러개의 도커 컨테이너들을 사용 할 수 있습니다.
백그라운드에서 데몬으로 돌도록 하려면 -d 옵션을 붙입니다.

### 웹에 띄울 html 파일 생성

보통 웹으로 띄울 html 파일을 아파치 서버에 추가하고 싶을 땐, Dockerfile의 ADD / COPY를 통해 호스트의 파일을 아파치 서버로 옮기는 명령어를 작성한 후 빌드합니다.

이렇게 하면 호스트의 html 파일과 아파치 서버의 html 파일이 동기화 되어 있지 않기 떄문에 매번 build를 해줘야하므로 번거롭습니다. 따라서 도커 볼륨을 사용해 호스트의 html 파일을 만들어놓고, 도커 컨테이너에서 그 파일에 접근해서 사용하는 방법을 활용합니다.

우선 호스트에 index.html 파일을 생성 및 수정합니다.

```shell
cd ~
mkdir html && cd html
vi index.html
```

```shell
Hello world
```

### Image로 Container를 생성

이제 컨테이너 생성 시 도커 볼륨을 통해 host와 도커 컨테이너의 html 폴더를 동기화 하겠습니다.

```shell
docker run -d -p 80:8080 [이미지명]
```

- --name : 컨테이너 이름
- -d : 백그라운드모드로 실행
- -p : [호스트포트][컨테이너포트] 포트 연결
- -v : 로컬과 컨테이너 파일 연동

### 접속 확인

Public DNS를 80번 포트로 접속해 접속을 확인합니다.

# 참고한 사이트

[https://wooono.tistory.com/123](https://wooono.tistory.com/123)
[https://www.yalco.kr/36_docker/](https://www.yalco.kr/36_docker/)
[https://sas-study.tistory.com/425](https://sas-study.tistory.com/425)
