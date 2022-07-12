# 서론

# 본론

## Apache

MPM 방식으로 HTTP 요청을 처리합니다. 기본적인 작동 방법은 사용자의 HTTP 요청이 올 때마다 프로세스나 스레드를 새로 만든다. 
HTTP 연결 요청이 올 때마다 프로세스를 매번 복제하여 프로세스에서 싱글 스레드로 해당 HTTP 요청을 처리합니다. 

### 구동방식

1. Prefork MPM(multi Processing Module) 방식

- HTTP 연결 요청이 올 때마다 프로세스를 매번 복제하여 프로세스에서 싱글 스레드로 해당 HTTP 요청을 처리합니다. 

2. Worker MPM 방식(멀티 스레드)

- 한 개의 프로세스가 여러 스레드를 생성하여 해당 HTTP 요청을 처리하므로 Prefork 방식보다 메모리 소모가 적습니다. 하지만 멀티 스레드 방식은 CPU 스케줄링을 위한 처리 시간과 문맥 전환 비용이 발생하는 단점이 있습니다.

## Nginx 

apache 의 C10K 문제점 해결을 위해 만들어진 Event-Driven 구조의 웹서버 SW 입니다. Nginx는 요청에 응답하기 위해 비동기 이벤트 기반 구조를 가진다. 이것은 아파치 HTTP 서버의 스레드/프로세스 기반 구조를 가지는 것과는 대조적입니다. Nginx의 비동기 이벤트란 HTTP 요청이 수 천 개여도 정해진 수의 프로세스(worker procss)가 이 요청들을 이벤트로 등록하고 비동기 방식으로 대기시켜 완료되는 요청(이벤트)부터 응답(처리)해주는 것을 말합니다. 

### 구동방식

NGINX에서는 커넥션 생성 및 커넥션 제거, 그리고 새로운 요청을 처리하는 것을 이벤트라고 부릅니다. 이 이벤트들은 OS커널이 큐 형식으로 worker 프로세스에게 전달해주고, 이벤트가 큐에 담긴 상태에서 worker 프로세스가 처리할 때까지 비동기 방식으로 대기합니다. 

### Nginx와 Apache 차이 이해하기 


# 참고한 사이트

[https://bentist.tistory.com/80](https://bentist.tistory.com/80)
[https://hyeon9mak.github.io/nginx-vs-apache/](https://hyeon9mak.github.io/nginx-vs-apache/)
[https://www.whatap.io/ko/blog/43/](https://www.whatap.io/ko/blog/43/)
[https://stackoverflow.com/questions/10829402/how-to-start-nginx-via-different-portother-than-80](https://stackoverflow.com/questions/10829402/how-to-start-nginx-via-different-portother-than-80)