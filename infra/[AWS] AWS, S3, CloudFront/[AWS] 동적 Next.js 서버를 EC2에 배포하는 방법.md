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

```
pm2 start yarn -w -i 0 --name "next" -- start
```

### 

하하

# 참고한 사이트

(https://github.com/nvm-sh/nvm)[https://github.com/nvm-sh/nvm]

