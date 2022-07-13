# 서론

# 본론
## 기본적인 명령어 
### 상태확인

```
systemctl status apache2.service
```

ProxyRequests
ProxyPreserveHost :::

### 재실행

```
service apache2 restart
```

### 네트워크 테스트 방법

```
PING 192.1.6.1
```

### 실행중인 포트확인

```
netstat -nlpt
```

## 리버스 프록시

### 프록시란

프록시(proxy)의 사전적 의미는 '대신', '대리'이다. 말 그대로 두 PC가 통신할 때, 직접 하지 않고 중간에서 대리로 통신하는 것을 `프록시`라고 하고, 중계 역할을 하는 것을 '프록시 서버'라고 부른다. 즉, 클라이언트와 서버 사이의 '중계 서버'라고 생각하면 된다. 프록시 서버는 보안 목적이나 캐싱 등의 기능을 제공합니다. 프록시 서버가 중간에 위치함으로써 클라이언트는 프록시 서버를 ‘서버’라고 인식하고, 서버 입장에서는 프록시 서버를 ‘클라이언트’로 인식하게 됩니다.

### 포워드 프록시란

일반적으로 프록시라고 말하면 포워드 프록시를 말합니다. 클라이언트에서 서버로 리소스를 요청할 때 직접 요청하지 않고 프록시 서버를 거쳐서 요청합니다. 이 경우 서버에서 받는 IP는 클라이언트의 IP가 아닌 프록시 서브의 IP이기 때문에 서버는 클라이언트가 누군지 알 수 없습니다. 즉, **서버에게 클라이언트가 누구인지 감춰주는 역할**을 합니다. 이러한 특징 때문에 기업 사내망에서 주로 사용됩니다.

### 리버스 프록시란 

리버스 프록시는 포워드 프록시와 반대 개념입니다. 애플리케이션 서버의 앞에 위치하여 클라이언트가 서버를 요청할 때 리버스 프록시를 호출하고, 리버스 프록시가 서버로부터 응답을 전달받아 다시 클라이언트에게 전송하는 역할을 합니다. 이 경우, 클라이언트는 애플리케이션 서버를 직접 호출하는 것이 아니라 프록시 서버를 통해 호출하기 때문에 리버스 프록시는 애플리케이션 서버를 감추는 역할을 하게 됩니다.

### 설정 방법

```
<VirtualHost *:443>

  ServerName domain.com
  ServerAlias <원하는 주소> ex)blog.domain.com or www.domain.com...
  ServerAdmin <email주소>

  ProxyRequests Off
  SSLProxyEngine on
  ProxyPreserveHost On
  AllowEncodedSlashes NoDecode

  <Proxy *>
    Order deny,allow
    Allow from all
  </proxy>

  SSLEngine on
  SSLProxyVerify none
  SSLProxyCheckPeerCN off
  SSLProxyCheckPeerName off
  SSLProxyCheckPeerExpire off
  SSLCertificateFile "<인증서 경로>/cert.pem"
  SSLCertificateKeyFile "<인증서 경로>/privkey.pem"
  SSLCertificateChainFile "<인증서 경로>/chain.pem"

  ProxyPass / http://127.0.0.1:8080/
  ProxyPassReverse / http://127.0.0.1:8080/

  RequestHeader set X-Forwarded-Proto "https"
  RequestHeader set X-Forwarded-Port "443"

  ErrorLog <로그 경로>/docker-error.log
  CustomLog <로그 경로>/docker-access.log combined

</VirtualHost>
```

- VirtualHost 

web Server는 Main Host외의 별도의 가상 Host를 사용하여 1개의 서버에서 여러개의 웹서버를 운영할 수 있습니다.
WAS를 연동할 때는 DocumentRoot를 사용하는 대신 RerverseProxy를 사용합니다. 

- 도메인 세팅
ServerName은 서비스할 도메인이며 ServerAlias는 서버의 별칭이다.

- 로그 세팅
ErrorLog는 에러로그의 경로이며, CustomLog는 접속 로그의 경로이다.

- 프록시 세팅

`proxyRequests Off`. on일 경우 Forward Proxy로 동작, off일 경우 Reverse Proxy로 동작하는 옵션이다.
`proxyPreserveHost On`. HTTP 요청 헤더의 Host: 부분을 유지하는 옵션이다.
`AllowEncodedSlaxhes NoDecode`. 웹 서버 뒤에서 인코딩된 /를 디코딩 하지 않도록 설정하는 옵션이다.
`<proxy *> Order deny,allow Allow from all </proxy>`. 프록시에 대한 보안 설정이다.
deny조건을 먼저 확인한 후 allow조건을 확인하며, 모든 호스트에서 접속이 가능하다.
`proxypass /<이곳에 경로를 설정해도 된다. ex) blog> http://127.0.0.1:8080/`. 위 세팅을 기준으로 설명했을 때, 외부에서 들어온 www.domin.com/blog/posts/1 요청을
127.0.0.1:8080/posts/1로 변환시켜주는 기능이다. 
`proxypassReverse /<이곳에 경로를 설정해도 된다 ex)blog> http://127.0.0.1:8080/`. 위의것과 기능은 동일하지만, 내부에서 리다이렉트가 일어났을시 생성되는 URL의 도메인이 127.0.0.1:8080/이
되버리기 때문에 이를 다시 www.domain.com/으로 변환해주는 기능이다. 

# 참고한 사이트

[https://webdir.tistory.com/196](https://webdir.tistory.com/196)
[https://chairsitman17.tistory.com/6](https://chairsitman17.tistory.com/6)
[https://velog.io/@always0ne/Proxy-Pass%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-Apache-Web-Server%EC%97%90-WAS-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0](https://velog.io/@always0ne/Proxy-Pass%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-Apache-Web-Server%EC%97%90-WAS-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)
