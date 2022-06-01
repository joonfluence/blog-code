# 서론

오늘은 Spring Boot 환경에서 MySQL과 MyBatis 설정을 하는 방법에 관해서 알아보도록 하겠습니다.

# 본론

### MySQL 설치방법

Mac을 사용한다면, 간단하게 brew로 mysql을 설치해줍니다.

```
brew install mysql
```

버젼도 확인해봅니다.

```
mysql -V
```

서버도 실행해봅니다.

```
mysql.server start
```

그리고 MySQL 초기 설정을 해줍니다. 비밀번호, 익명유저, root 접속 권한 등을 설정해줍니다.

```
mysql_secure_installation
```

그런 뒤, 접속해줍니다.

```
mysql -u root -p
```

다시 mysql 서버를 실행하고 설정을 켜줍니다.

# 참고한 사이트

https://shanepark.tistory.com/41
