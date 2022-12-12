### PHP 화면 상 태그 지우는 방법

```shell
php --ini | grep php.ini
```

php.ini 파일에서 short_open_tag 속성 값을 Off에서 On으로 변경

```ini
short_open_tag = On
```
