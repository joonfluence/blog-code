### Node.js 웹 앱의 도커라이징

- Node.js 앱 생성

이 디렉터리 안에 애플리케이션과 의존성을 알려주는 package.json 파일을 생성하겠습니다.

```javascript
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

- 서버 구성

```javascript
"use strict";

const express = require("express");

// 상수
const PORT = 8080;
const HOST = "0.0.0.0";

// 앱
const app = express();
app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(PORT, HOST, () => {
  console.log(`Running on http://${HOST}:${PORT}`);
});
```

- Dockerfile 생성

```shell
touch Dockerfile
```

dockerfile을 생성해줍니다.

```
FROM node:16

# 앱 디렉터리 생성
WORKDIR /usr/src/app

# 앱 의존성 설치
# 가능한 경우(npm@5+) package.json과 package-lock.json을 모두 복사하기 위해
# 와일드카드를 사용
COPY package*.json ./

# RUN npm install
# 프로덕션을 위한 코드를 빌드하는 경우
RUN npm ci --only=production

# 앱 소스 추가
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

- .dockerignore 파일 작성

```
node_modules
npm-debug.log
```

- 이미지 빌드

```
docker build . -t <your username>/node-web-app
```

- 이미지 확인

```
docker images

REPOSITORY                      TAG        ID              CREATED
node                            16         1934b0b038d1    5 days ago
<your username>/node-web-app    latest     d64d3505b0d2    1 minute ago
```

- 이미지 실행

```
docker run -p 49160:8080 -d <your username>/node-web-app
```

- 앱 로그 확인

```
# 컨테이너 아이디를 확인합니다
$ docker ps

# 앱 로그를 출력합니다
$ docker logs <container id>

# 예시
Running on http://localhost:8080
```

- exec 명령어

컨테이너 안에 들어가 봐야 한다면 exec 명령어를 사용할 수 있습니다.

```
# 컨테이너에 들어갑니다
$ docker exec -it <container id> /bin/bash
```

- 테스트

```
$ curl -i localhost:49160

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
Date: Mon, 13 Nov 2017 20:53:59 GMT
Connection: keep-alive

Hello world
```

# 참고한 사이트

[https://nodejs.org/ko/docs/guides/nodejs-docker-webapp/#dockerignore](https://nodejs.org/ko/docs/guides/nodejs-docker-webapp/#dockerignore)
