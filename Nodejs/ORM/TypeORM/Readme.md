### 설치방법

```
npm install typeorm reflect-metadata --save
```

import it somewhere in the global place of your app (for example in app.ts):

```
import "reflect-metadata"
```

### NestJS에서 사용하는 방법

- 환경설정

```
npm install --save @nestjs/typeorm typeorm mysql2
```

[https://docs.nestjs.com/techniques/database](https://docs.nestjs.com/techniques/database)
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
[https://docs.nestjs.com/modules](https://docs.nestjs.com/modules)
[https://docs.nestjs.com/fundamentals/custom-providers](https://docs.nestjs.com/fundamentals/custom-providers)
[https://anywaydevlog.tistory.com/40](https://anywaydevlog.tistory.com/40)
[https://medium.com/@vahid.vdn/nestjs-providers-usevalue-useclass-usefactory-63a71f94da43](https://medium.com/@vahid.vdn/nestjs-providers-usevalue-useclass-usefactory-63a71f94da43)
[https://medium.com/crocusenergy/nestjs-modules-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%8B%A4%EC%8A%B5-758b1328e9e7](https://medium.com/crocusenergy/nestjs-modules-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%8B%A4%EC%8A%B5-758b1328e9e7)
[https://medium.com/crocusenergy/nestjs-middleware-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%8B%A4%EC%8A%B5-649b14bf65ff](https://medium.com/crocusenergy/nestjs-middleware-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%8B%A4%EC%8A%B5-649b14bf65ff)
[https://www.wisewiredbooks.com/nestjs/overview/05-modules.html](https://www.wisewiredbooks.com/nestjs/overview/05-modules.html)
[https://www.youtube.com/watch?v=3JminDpCJNE&ab_channel=JohnAhn](https://www.youtube.com/watch?v=3JminDpCJNE&ab_channel=JohnAhn)
[https://velog.io/@chappi/Nestjs%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-9%EC%9D%BC%EC%B0%A8-Repository%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Board-CRUD](https://velog.io/@chappi/Nestjs%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-9%EC%9D%BC%EC%B0%A8-Repository%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Board-CRUD)
