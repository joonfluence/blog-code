### 설치방법

```
npm install typeorm reflect-metadata --save
```

import it somewhere in the global place of your app (for example in app.ts):

```
import "reflect-metadata"
```

### NestJS에서 사용하는 방법

[https://docs.nestjs.com/recipes/sql-typeorm](https://docs.nestjs.com/recipes/sql-typeorm)

### 테이블이 생성된 상태에서 Entity 생성하는 방법

```
npm i -g typeorm-model-generator
yarn add typeorm-model-generator
```

entity 파일 생성 명령어는 아래와 같다.

- -h : host, 연결할 서버 ip
- -d : database, 연결할 db 이름
- -p : port, 연결할 서버 port
- -u : user, db 사용자 id
- -x : db 사용자 패스워드
- -e : engine, db 종류 (mssql, postgres, mysql, mariadb, oracle, sqlite)
- -o : out, entity 파일 생성할 폴더 경로

```
typeorm-model-generator -h server_ip -d database_name -p server_port -u server_id -x server_pw -e db_종류 -o entity_생성할_folder_경로
```

# 참고한 사이트

[https://typeorm.io/](https://typeorm.io/)
[https://anywaydevlog.tistory.com/40](https://anywaydevlog.tistory.com/40)
