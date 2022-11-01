### 기본 설정 파일

- ec2 내부에 /etc/apache2 폴더에 설정 파일등이 존재

⇒ apache2.conf 에서 설정 추가

```shell
<Directory /var/www/html/kurrant_flutter_web_v2/*>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride All
    Require all granted
    RewriteEngine on
</Directory>
```

- /etc/apache2/site-available 로 이동

⇒ 000-default.conf와 kurrant-ssl.conf 편집

```shell
// 000-default.conf
//이전
DocumentRoot /var/www/html/

//이후
DocumentRoot /var/www/html/kurrant_flutter_web_v2/build/web

// kurrant-ssl.conf
//이전
DocumentRoot /var/www/html/

//이후
DocumentRoot /var/www/html/kurrant_flutter_web_v2/build/web
```

- sudo a2enmod rewrite 모듈 허용
- 설정 전부 변경후 sudo service apache2 restart를 통해 아파치 서버 재시작
- kurrant_flutter_web_v2/build/web 밑에 .htaccess 파일생성

```shell
// 클라이언트에서 처리하는 방법
Options -MultiViews
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.html [QSA,L]

// 서버에서 처리하는 방법
// apache2.conf 안에 다음과 같이 설정을 넣어줌
<Directory "/var/www/html">
	Options -MultiViews
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.html [QSA,L]
    Require all granted
</Directory>
```

- dev서버 포트

ec2 인바운드규칙에서 3003, 3005 추가

- apache 상태 확인

```shell
systemctl status apache2.service
```

### SSL 및 프록시 설정 하는 방법

/etc/apache2/sites-available의 000-default.conf 설정 파일에 가보자.

```shell
<VirtualHost *:80>
ServerName makers-admin.kurrant.co
Redirect permanent / https://makers-admin.kurrant.co/
	<Directory /var/www/html/makers-admin>
		Options MultiViews
		AllowOverride All
		Order allow,deny
		Allow from all
	</Directory>
</VirtualHost>
```

kurrant-ssl.conf 파일에서는 ssl 설정과 프록시 설정을 아래와 같이, 해줄 수 있다.

```shell
<VirtualHost *:443>
	ServerName makers-admin.kurrant.co
	DocumentRoot "/var/www/html/makers-admin"
	ProxyRequests Off
	ProxyPreserveHost on
	ProxyPass / http://127.0.0.1:3002/
	ProxyPassReverse / http://127.0.0.1:3002/
	RequestHeader set X-Forwarded-Proto "https"
	RequestHeader set X-Forwarded-Port "443"
	SSLEngine on
	SSLCertificateFile "/var/www/html/keys/_wildcard_kurrant_co.crt"
	SSLCertificateKeyFile "/var/www/html/keys/_wildcard_kurrant_co_SHA256WITHRSA.key"
	SSLCertificateChainFile "/var/www/html/keys/ChainCA/rsa-dv.chain-bundle.pem"
	SSLCACertificateFile "/var/www/html/keys/ChainCA/AAACertificateServices.crt"
</VirtualHost>
```
