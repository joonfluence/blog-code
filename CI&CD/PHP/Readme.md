### 서론

오늘은 PHP 프로그램에서 Github Action을 통한 CI/CD를 구축하는 방법에 관해서 알아보도록 하겠습니다.

### 방법

appleboy/ssh-action@master를 사용한다.

```yaml
name: Build STAGING PEOPET

on:
  push:
    branches: ["staging"]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: ssh EC2
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.KEY }}
          port: 22
          script: |
            cd ${{ secrets.HOME }}
            git pull newOrigin staging
```

EC2를 접속하고 아래 script를 자동으로 실행해준다. 단순히 EC2에 접속하여, 지정된 Github Repository에서 정보를 불러오면 끝난다.

### 참조한 포스트

[https://github.com/appleboy/ssh-action](https://github.com/appleboy/ssh-action)
