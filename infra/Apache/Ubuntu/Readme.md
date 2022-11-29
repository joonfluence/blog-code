# 서론

# 본론

## 기본적인 명령어

### 웹서버 상태 확인

```shell
systemctl status apache2.service
```

### 네트워크 테스트 방법

```shell
PING 192.1.6.1
```

### 서버에서 실행 중인 포트확인

```shell
netstat -nlpt
```

### 크론 탭 상태 확인

```shell
service cron status
```

### pid 바탕으로 프로세스 조회하는 방법

```shell
ps -ef | grep 'Process Name'
```