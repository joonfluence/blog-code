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
# server image는 ubunutu 18.04를 사용
FROM ubuntu:18.04 
# Dockerfile 작성자
MAINTAINER Wimes <yms04089@kookmin.ac.kr> 

# image가 올라갔을 때 수행되는 명령어들
# -y 옵션을 넣어서 무조건 설치가 가능하도록 한다.
RUN \
    apt-get update && \
    apt-get install -y apache2

# apache가 기본적으로 80포트를 사용하기 때문에 expose를 이용해 apache server로 접근이 가능하도록 한다.
EXPOSE 80 

# 컨테이너가 생성 된 이후에 내부의 아파치 서버는 항상 실행중인 상태로 만들어준다.
# apachectl을 foreground(즉, deamon)상태로 돌아가도록 한다.
CMD ["apachectl", "-D", "FOREGROUND"]
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

### 생성한 Dockerfile을 Image로 빌드하는 방법

```shell
docker build -t [이미지 이름:이미지 버전] [Dockerfile의 경로]
```

```shell
docker build -t apache-image .
```

```shell
docker images
```

생성된 이미지를 확인해줍니다. 

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
docker run --name apache-container -d -p 80:80 -v ~/html/:/var/www/html apache-image
```

- --name : 컨테이너 이름
- -d : 백그라운드모드로 실행
- -p : [호스트포트][컨테이너포트] 포트 연결
- -v : 로컬과 컨테이너 파일 연동

### 접속 확인

Public DNS를 80번 포트로 접속해 접속을 확인합니다.

# 참고한 사이트

[https://wooono.tistory.com/123](https://wooono.tistory.com/123)
