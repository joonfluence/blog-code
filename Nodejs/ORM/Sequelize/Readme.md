# 본론

### 환경설정

- 패키지 설치하기

```shell
npm i sequelize sequelize-cli mysl2
```

- CLI 명령어 입력하기

```shell
npx sequelize init
```

자동으로 config, modles, migrations, seeders 폴더가 생성된다.

- MySQL 연결하기

```javascript
sequelize.sync({ force: false });
```

### 모델 정의하기

### 관계 정의하기

- 1:1
- 1:N
- N:M

### 쿼리 사용하기

- findOne
- findAll
- create
- update
- destory
