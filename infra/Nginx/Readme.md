### nginx 기본 명령어

```shell
nginx -t					# nginx 설정 파일의 문법이 올바른지 확인

sudo service nginx status 	# nginx 상태 확인
sudo service nginx start	 # nginx 실행
sudo service nginx restart 	# 중지 후 재실행
sudo servcie nginx reload	 # 수정된 파일 적용하여 연결을 끊지 않고 재실행
sudo service nginx stop		 # nginx 중지

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

# 참고한 사이트

[https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0](https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
