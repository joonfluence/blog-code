# 서론

오늘은 PHP(Hypertext Preprocessor)란 언어의 특성과 PHP를 설치하는 방법에 관해 알아보도록 하겠습니다.

# 본론

WAMP(Window Apache MySQL PHP) vs MAMP(Mac Apache MySQL PHP)

WAMP packaged by Bitnami provides a complete, fully-integrated and ready to run WAMP development environment. In addition to PHP, MySQL and Apache, it includes FastCGI, OpenSSL, phpMyAdmin, ModSecurity, SQLite, ImageMagick, xDebug, Xcache, OpenLDAP, ModSecurity, Memcache, OAuth, PEAR, PECL, APC, GD, cURL and other components and the following frameworks:Zend Framework, Symfony, CodeIgniter, CakePHP, Smarty, Laravel.

### PHP 8.0 버젼 설치하기

```
brew install php@8.0
```

### PHP 기본 버젼으로 설정하기

```
brew link --overwrite --force php@8.0
```

설명처럼 아래 두 명령어를 실행하여 \*shrc 파일을 업데이트합니다.

```shell
echo 'export PATH="/usr/local/opt/php@8.0/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="/usr/local/opt/php@8.0/sbin:$PATH"' >> ~/.zshrc
```

### PHP 서버 실행하는 방법

```json
cd path/to/your/app
php -S 127.0.0.1:8000
```

# 참고한 사이트

[https://velog.io/@timcodejs/PHP-MacOS%EC%97%90%EC%84%9C-PHP-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0](https://velog.io/@timcodejs/PHP-MacOS%EC%97%90%EC%84%9C-PHP-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
[https://blog.gilbok.com/how-to-install-php-7-4-and-set-as-default-on-mac/](https://blog.gilbok.com/how-to-install-php-7-4-and-set-as-default-on-mac/)
