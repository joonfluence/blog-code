### Apache에서 SSL 설정 방법

```shell
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

Web Server는 Main Host외의 별도의 **가상 Host**를 사용하여 _1개의 서버에서 여러 개의 웹서버를 운영할 수 있습니다. WAS를 연동할 때는 DocumentRoot를 사용하는 대신 RerverseProxy를 사용합니다.

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