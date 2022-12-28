### Spring에서 로그 남기는 방법

Spring 서버에선 임시로 로그를 저장하나, 실제 디스크에 남기기 위해선 application.yaml 파일에 다음과 같이 기록해줘야 한다.

```yaml
logging:
  file:
    name: my.log
    path: /var/log
```
