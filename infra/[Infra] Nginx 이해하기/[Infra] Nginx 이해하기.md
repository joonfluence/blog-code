### nginx 기본 명령어

```shell
nginx -t					# nginx 설정 파일의 문법이 올바른지 확인

sudo service nginx status 	# nginx 상태 확인

sudo service nginx start	# nginx 실행
sudo service nginx restart	# 중지 후 재실행
sudo servcie nginx reload	# 수정된 파일 적용하여 연결을 끊지 않고 재실행
sudo service nginx stop		# nginx 중지

# 기본적으로 Nginx는 서버가 부팅될 때 자동으로 시작됩니다.
sudo service disable nginx	# 자동 시작 비활성화
sudo service enable nginx	# 자동 시작 활성화
```

### nginx 설치 방법

```shell
sudo apt-get update
sudo apt install nginx -y
nginx -v
sudo service nginx status
```

Nginx 파일 및 디렉토리
콘텐츠
/var/www/html : 기본적으로 기본 Nginx 페이지가 있습니다. Nginx 구성 파일을 변경 수 있습니다.

서버 구성
/etc/nginx/nginx.conf : 기본 Nginx 구성 파일입니다. Nginx 전역 구성을 변경하도록 수정할 수 있습니다.
/etc/nginx/sites-available/ : 사이트별로 서버 블록을 저장할 수 있는 디렉토리입니다. Nginx는 sites-enabled 디렉토리에 연결되지 않은 경우 이 디렉토리의 구성 파일을 사용하지 않습니다. 일반적으로 sites-available 디렉토리에서 서버 블록이 구성된 후에 다른 디렉토리에 연결되어 사용할 수 있게 됩니다.
/etc/nginx/sites-enabled : 활성화된 사이트별 서버 블록이 저장되는 디렉토리입니다. 일반적으로 sites-available 디렉토리에 있는 구성파일에 연결해 생성됩니다.

서버 로그
/var/log/nginx/access.log : 별도로 구성되지 않는 한 웹 서버에 대한 모든 요청이 기록됩니다.
/var/log/nginx/error.log : 모든 Nignx 오류가 기록됩니다.

ngingx.conf
우선 sudo cat /etc/nginx/nginx.conf을 실행해 기본 Nginx 구성 파일을 살펴보겠습니다.

# 참고한 사이트

[https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0](https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)