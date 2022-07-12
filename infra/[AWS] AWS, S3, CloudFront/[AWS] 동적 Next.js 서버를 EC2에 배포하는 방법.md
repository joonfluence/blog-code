# 서론

오늘은 Next.js 서버를 EC2에 배포하는 방법에 관해 알아보도록 하겠습니다. 

### EC2에 배포해야 하는 까닭

Next.js는 S3에 배포할 수 있는데도 할 수 있습니다. 그러나 getServerSideProps와 같이, SSR 기술을 프로젝트에서 사용했을 경우에는 내부적으로 express를 사용하고 있으므로 EC2에 배포해야 합니다. 

### 서버 설정 방법

다음 프로그램이 EC2 인스턴스에 설치되었는지 확인해야 합니다. 

- Node.js 
- Package manager (npm, yarn) 
- pm2 (Node 프로세스 관리 도구) 
- NGINX (리버스 프록시 및 HTTPS 설정) 
- Cerbot (Let's Encrypt 인증서 발급 도구) 

Node.js는 Node Version Manager를 통해서 설치하는 것이 편리하므로 NVM을 먼저 설치해줘야 합니다. 아래 curl 명령어를 통해, 쉽게 설치할 수 있습니다. 

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1[버젼명]/install.sh | bash
```

참고로 curl 은 command line 용 `data transfer tool`입니다. download/upload 모두 가능하며, `HTTP/HTTPS/FTP/LDAP/SCP/TELNET/SMTP/POP3` 등 주요한 프로토콜을 지원하며 Linux/Unix 계열 및 Windows 등 주요한 OS에서 구동되므로 여러 플랫폼과 OS에서 유용하게 사용할 수 있단 장점을 가진 도구입니다.

```shell
nvm install 16.16.0
nvm use 16.16.0
```

성공적으로 설치됐다면, LTS가 지원되는 node.js의 버젼을 설치해줍니다. node.js를 설치하면 기본 패키지 매니저인 npm이 자동으로 설치됩니다. 만약 yarn을 설치해야 하는 경우라면, yarn도 설치해줍니다. 

```shell
npm i -g yarn
yarn --version 
```

그런 뒤, Node.js 프로세스 관리 도구인 pm2를 설치해줍니다. 

```shell
npm i -g pm2
```

그런 뒤, 배포할 프로젝트 파일을 가져와줍니다. 

```shell
git clone [프로젝트 URL]
```

프로젝트의 의존성 패키지들을 설치해줍니다. 

```shell
yarn
# or
npm i 
```

그리고 Next.js 프로젝트를 빌드해줍니다. 

```shell
yarn build
# or 
npm run build 
```

pm2를 통해서 프로젝트 배포를 시작해줍니다. 

```shell
pm2 start yarn -w -i 0 --name "next" -- start
```

### NGINX 리버스 프록시, 도메인, HTTPS 적용하기 

- 리버스 프록시를 적용하면, 3000 포트에서 실행되는 서버를 80 포트로 배포하도록 처리할 수 있다. 
- WAS에서 서버 설정을 할 경우, 설정 방법이 비교적 복잡하기 때문에 NGINX 리버스 프록시를 적용하는 것이 좋다. 
- 리버스 프록시를 적용하면 NGINX의 기능을 사용할 수 있기 때문에, NGINX를 통해 서버 설정을 좀 더 편하게 할 수 있습니다. 예를 들어, 도메인 설정이나 HTTPS 적용 등
- WAS를 그대로 배포했을 때, DB 서버가 노출될 수 있는 문제도 막을 수 있습니다. 

1. EC2 인스턴스의 퍼블릭 IP를 가지고 도메인을 설정해줍니다. 도메인을 구입한 사이트에서 A 레코드에 해당 IP 주소를 입력하고 저장해줍니다. 
2. NGINX를 설치합니다. 

```shell
sudo apt-get install nginx -y
nginx -v # 잘 설치되었는지 확인한다.
```

3. NGINX 설정을 위한 파일을 편집하기 위해 설정 파일을 엽니다. 여기서는 기본 생성된 설정 파일인 `etc/nginx/sites-available/default`를 열었습니다.

```shell
sudo vim /etc/nginx/sites-available/default
```

4. 내용을 모두 지우고, 다음과 같이 리버스 프록시 설정을 해줍니다. 

```shell
server {
  listen 80 default_server;
  listen [::]:80 default_server;

  server_name <도메인 주소>;

  location / {
          proxy_pass http://127.0.0.1:3000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```

5. 다음 명령어로 NGINX 설정이 올바른지 확인한 다음, NGINX 서비스를 재시작해줍니다. 

```shell
sudo nginx -t
sudo systemctl reload nginx
```

6. 이제 웹 브라우저를 통해 설정한 도메인 주소로 접속해보면 페이지가 잘 나오는 것을 확인할 수 있습니다. 마지막으로 HTTPS를 적용해줍니다.
7. Certbot을 설치해줍니다.

```shell
sudo apt-get install python3-certbot-nginx
```

8. 인증서 발급을 시작합니다. 

```shell
sudo certbot --nginx
```

9. 마지막으로 인증서가 만료되기 전에 알아서 갱신되도록 Crontab을 설정할 것이다. 먼저 Crontab 설정 파일을 연다. 설정 파일을 열 때 편한 편집기를 고르라는 메시지가 뜨는데, 편한 편집기로 고르면 된다.

```shell
sudo crontab -e
```

10. 최하단에 다음 설정을 추가한다. 이렇게 설정하면 매월 1일 오후 6시마다 인증서를 갱신하고 NGINX를 재시작하는 명령어가 실행된다.

```shell
0 18 1 * * certbot renew --renew-hook="sudo systemctl restart nginx"
```
# 참고한 사이트

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)
[https://puterism.com/deploy-next-js-with-ec2/](https://puterism.com/deploy-next-js-with-ec2/)
[https://may9noy.tistory.com/194](https://may9noy.tistory.com/194)
[https://dev.to/tikam02/how-to-deploy-node-server-on-apache2-23d7](https://dev.to/tikam02/how-to-deploy-node-server-on-apache2-23d7)
