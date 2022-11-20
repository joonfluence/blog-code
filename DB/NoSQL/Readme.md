### 목차

1. NoSQL vs SQL
2. 종류

- MongoDB
  - 설치하기
    - MongoDB
    - 컴퍼스 
    - 몽구스 
  - 데이터베이스 및 컬렉션 생성하기
  - CRUD 작업하기
  - ODM 
    - 몽구스 

### 기본작업

- 서버 시작 요청
  mongod
- 셸(자바스크립트 해석기) 시작
  mongo

### DB & 콜렉션 다루기

show [DB명 Or 콜렉션명]
use [DB명 Or 콜렉션명]
Database 생성: use [DB명]
Database 제거: db.dropDatabase();
콜렉션 생성 : db.createCollection();
콜렉션 제거 : db.COLLECTION_NAME.drop();
help
exit

### 몽고디비 셸 사용하기

mongo 호스트명:포트번호/DB명 : 로컬이 아닌, 다른 장비 또는 포트에 mongod를 연결할 때.
