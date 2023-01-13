### 서론

오늘은 웹 서버로 자주 사용되는 Nginx 설정에 대해서 알아보도록 하겠습니다.

### 웹서버란

엡 서버란 이미지 동영상, 자바스크립트, HTML 등 다양한 문서를 제공하는 서버 시스템을 말함. 주로 HTTP 통신 프로토콜을 통해 리소스를 전달하지만 FTP, SMTP와 같은 다른 프로토콜도 지원.

[Nginx_web_server](./Nginx_web_server.png)

웹 서버는 데이터 전송, 어플리케이션 실행, 프록시 처리 등의 역할을 담당.

1. 데이터 전송

- HTML 텍스트 파일을 비롯하여 이미지나 음성 데이터와 같은 정적 콘텐츠를 웹 클라이언트에 전송.
- 이를 이용해 CSR(React, Vue, Angular 등)에 의해 생성된 빌드 파일(정적 파일)을 제공할 수 있음.

2. 어플리케이션 실행

- PHP와 같은 모듈을 내장해서 웹 서버가 직접 Application Server를 실행할 수 있음.

3. 프록시 처리

- 클라이언트의 요청을 Application Server로 전달하는 역할을 함.
- 이를 이용해 캐시 처리를 할 수 있고 로드 밸런싱 기능, 암호화 기능 등 처리할 수 있으며, 웹 서버가 사용되는 가장 큰 이유이기도 함.

### Nginx 특징

비동기(Event Driven)에 의한 Non Blocking 처리를 한다는 것. 그에 따라 동시 접속사가 늘어날수록 물리 메모리가 증가하는 프로세스 기반의 apache 서버에 의해 소비 메모리량이 적어지면서 동시 처리수를 급격하게 늘릴 수 있음.

또한 single Thread 기반으로 마스터 / worker 프로세스 구동 방식을 채택하여 context switching 하지 않아, CPU 사용률을 감소시킬 수 있음.

[Nginx_single_thread](./Nginx_single_thread.png)

다만 하드웨어 자원을 사용하는 것이므로 nginx에서 읽기/쓰기가 자주 일어난다면 아피치가 성능 상 더 좋을 수 있음. 하지만 대부분의 웹 서버에서는 하드웨어 읽기가 발생하지 않는 **캐시 제공, 리버스 프록시 서버, 로드 밸런서** 등의 역할을 주로 담당하게 되므로 최근들어 더 자주 사용된다고 생각할 수 있음. 그 외에도 nginx는 여러 기능을 **모듈** 단위로 개발하여 nginx를 컴파일할 때 필요한 모듈들만 조합해서 사용할 수 있음.

### nginx 설치하는 방법

- 우분투

```shell
# apt repository 에 설치하고자 하는 nginx 버전 추가
# ubuntu 버전 (18.04: bionic, 16.04: xenial)
sudo touch /etc/apt/sources.list.d/nginx.list
echo "deb http://nginx.org/packages/ubuntu/ bionic nginx" | sudo tee -a /etc/apt/sources.list.d/nginx.list
echo "deb-src http://nginx.org/packages/ubuntu/ bionic nginx"| sudo tee -a /etc/apt/sources.list.d/nginx.list

# 인증 키 등록
wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key

# 저장소 업데이트
sudo apt-get update

# nginx 설치
sudo apt-get install nginx
```

- centOS

```shell
# nginx 공식 저장소 추가
sudo vim /etc/yum.repos.d/nginx.repo
# 파일에 아래 내용 추가
  [nginx]
  name=nginx repo
  baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
  gpgcheck=0
  enabled=1

# nginx 설치
sudo yum install nginx
```

### nginx 기본 환경 변수

nginx 를 설치하면 /etc/nginx 에 아래와 같은 디렉토리가 나올 것

```shell
├── conf.d # nginx.conf 에서 불러들일수 있는 설정 파일
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── nginx.conf # 기본 설정 파일
├── proxy_params
├── scgi_params
├── sites-available # 가상 호스트 설정 파일들 위치
│   └── default
├── sites-enabled
│   └── default -> /etc/nginx/sites-available/default
├── snippets # nginx와 관련된 잡다한 설정파일들 위치 (ubuntu 에서는 여기에 위치시키는 것이 관례)
│   ├── fastcgi-php.conf
│   └── snakeoil.conf
├── uwsgi_params
└── win-utf
```

관련된 설정 파일을 열어보면 아래와 같습니다. 

```shell
# worker 프로세스를 실행할 사용자 설정
# - 이 사용자에 따라 권한이 달라질 수 있다.
user  nginx;
# 실행할 worker 프로세스 설정
# - 서버에 장착되어 있는 코어 수 만큼 할당하는 것이 보통, 더 높게도 설정 가능
worker_processes auto;

# 오류 로그를 남길 파일 경로 지정
error_log  /var/log/nginx/error.log warn;
# NGINX 마스터 프로세스 ID 를 저장할 파일 경로 지정
pid        /var/run/nginx.pid;

# 접속 처리에 관한 설정을 한다.
events {
    # 워커 프로레스 한 개당 동시 접속 수 지정 (512 혹은 1024 를 기준으로 지정)
    worker_connections  1024;
}

# 웹, 프록시 관련 서버 설정
http {
  # mime.types 파일을 읽어들인다.
  include       /etc/nginx/mime.types;
  # MIME 타입 설정
  default_type  application/octet-stream;

  # 엑세스 로그 형식 지정
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

  # 엑세스 로그를 남길 파일 경로 지정
  access_log  /var/log/nginx/access.log  main;

  # sendfile api 를 사용할지 말지 결정
  sendfile        on;
  #tcp_nopush     on;

  # 접속시 커넥션을 몇 초동안 유지할지에 대한 설정
  keepalive_timeout  65;

  # (추가) nginx 버전을 숨길 수 있다. (보통 아래를 사용해서 숨기는게 일반적)
  server_tokens off

  #gzip  on;

  # /etc/nginx/conf.d 디렉토리 아래 있는 .conf 파일을 모두 읽어 들임
  include /etc/nginx/conf.d/*.conf;
  server {
      listen       80;
      listen       [::]:80;
      server_name  _;
      root         /usr/share/nginx/html;

      # Load configuration files for the default server block.
      include /etc/nginx/default.d/*.conf;

	location / {
 	if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, DELETE, PATCH, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
            add_header 'Access-Control-Max-Age' 86400;
            return 204;
        }
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Content-Type' 'application/json' always;
        proxy_pass http://localhost:3000/;
	}

	error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
    server {
    listen 80;
    listen [::]:80;
    server_name [도메인명];
    root [서버 루트 디렉토리];

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php-fpm/www.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

```shell
# ubuntu
service nginx reload;

# centOS
systemctl start nginx
```

### Nginx 파일 및 디렉토리

- 콘텐츠

/var/www/html : 기본적으로 기본 Nginx 페이지가 있습니다. Nginx 구성 파일을 변경 수 있습니다.

- 서버 구성

/etc/nginx/nginx.conf : 기본 Nginx 구성 파일입니다. Nginx 전역 구성을 변경하도록 수정할 수 있습니다.
/etc/nginx/sites-available/ : 사이트별로 서버 블록을 저장할 수 있는 디렉토리입니다. Nginx는 sites-enabled 디렉토리에 연결되지 않은 경우 이 디렉토리의 구성 파일을 사용하지 않습니다. 일반적으로 sites-available 디렉토리에서 서버 블록이 구성된 후에 다른 디렉토리에 연결되어 사용할 수 있게 됩니다.
/etc/nginx/sites-enabled : 활성화된 사이트별 서버 블록이 저장되는 디렉토리입니다. 일반적으로 sites-available 디렉토리에 있는 구성파일에 연결해 생성됩니다.

-  서버 로그

/var/log/nginx/access.log : 별도로 구성되지 않는 한 웹 서버에 대한 모든 요청이 기록됩니다.
/var/log/nginx/error.log : 모든 Nignx 오류가 기록됩니다.

- ngingx.conf

우선 sudo cat /etc/nginx/nginx.conf을 실행해 기본 Nginx 구성 파일을 살펴보겠습니다.

# 참고한 사이트

[https://opentutorials.org/module/384/4526](https://opentutorials.org/module/384/4526)
[https://www.nginx.com/resources/wiki/start/topics/tutorials/install/](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)
[https://kscory.com/dev/nginx/install](https://kscory.com/dev/nginx/install)
