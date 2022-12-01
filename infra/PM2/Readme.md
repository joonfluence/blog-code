# 서론

오늘은 Node.js의 프로세스 매니저인 PM2에 관해 알아보겠습니다.

### 사용하는 이유

굳이 똑같은 앱을 많은 메모리를 사용해가며 PM2에서 Node.js를 실행하는 이유는 Node.js가 **싱글 스레드**여서 1개의 CPU만 사용하기 때문입니다. PM2는 Node.js의 **클러스터 모듈**를 사용해서 원하는 수 만큼 프로세스를 쉽게 늘리고 줄일 수 있도록 도와줍니다. 자연히 1개의 프로세스만 실행할 때보다 성능과 안정성을 높일 수 있습니다.

### 클러스터 모드

### 자주 사용하는 명령어

```shell
pm2 start [프로세스명]
```

```shell
pm2 stop [프로세스명]
```

```shell
pm2 restart [프로세스명]
```

```shell
pm2 monit
```

### 예시

```
pm2 start npm -w -i 0 --name "next" -- start
```

# 참고한 사이트

[https://engineering.linecorp.com/ko/blog/pm2-nodejs/](https://engineering.linecorp.com/ko/blog/pm2-nodejs/)
[https://blog.rhostem.com/posts/2019-10-23-pm2-graceful-reload](https://blog.rhostem.com/posts/2019-10-23-pm2-graceful-reload)
