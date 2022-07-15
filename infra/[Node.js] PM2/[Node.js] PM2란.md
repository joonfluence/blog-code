# 서론

오늘은 Node.js의 프로세스 매니저인 PM2에 관해 알아보겠습니다. 

### 자주 사용하는 명령어

```
pm2 start [프로세스명]
```

```
pm2 stop [프로세스명]
```

```
pm2 restart [프로세스명]
```

```
pm2 monit
```

### 예시 

```
pm2 start npm -w -i 0 --name "next" -- start
```

# 참고한 사이트

[https://engineering.linecorp.com/ko/blog/pm2-nodejs/](https://engineering.linecorp.com/ko/blog/pm2-nodejs/)

