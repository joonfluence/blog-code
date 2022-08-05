# 서론

오늘은 MariaDB(RDS)를 Spring Boot 프로젝트에 연동하는 방법에 관해서 알아보도록 하겠습니다. 

# 본론

### RDS 생성방법

AWS 로그인 > RDS > 대시보드 > 데이터베이스 생성 > 프리티어 > admin > 암호설정 > 퍼블릭엑세스 가능 > 데이터베이스 생성

### MySQL 워크벤치 설정방법

Hostname에 엔드포인트 작성 > Username에 admin 입력 > Store in Keychain에 RDS 비밀번호 입력 > OK


### RDS 연동하기 

application.properties 파일에 아래와 같이 입력해줌. 

```shell
# MariaDB Connector 의 클래스. DB 연결 드라이버 정의
private static final String DRIVER = "org.mariadb.jdbc.Driver";
# DB 경로
private static final String URL = "jdbc:mariadb://엔드포인트:포트번호/인스턴스이름";
private static final String USER = 사용자이름;
private static final String PASSWORD = 비밀번호;
```

# 결론

오늘은 MariaDB과 RDS를 연동하는 방법에 관해 알아보았습니다. 

# 참고한 사이트

[https://durian9s-coding-tree.tistory.com/15](https://durian9s-coding-tree.tistory.com/15)
[https://doing7.tistory.com/35](https://doing7.tistory.com/35)
