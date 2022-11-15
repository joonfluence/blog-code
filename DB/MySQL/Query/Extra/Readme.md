# 본문

### DB 조회/생성/삭제

```sql
show databases; -- DB 목록 조회
create database kurrant; -- 생성
drop database kurrant; -- 삭제
```

### 비밀번호 변경

```sql
set PASSWORD FOR 'root'@'localhost' = PASSWORD("1234");
```

### 칼럼 삭제하기

```sql
ALTER TABLE 테이블명 DROP 칼럼명;
ALTER TABLE 테이블명 DROP FOREIGN KEY 칼럼명;
```

### 칼럼 속성 수정하기

```sql
ALTER TABLE 테이블명 MODIFY COLUMN 칼럼명 int(2) DEFAULT 0 NULL;
```

### Primary Key 설정

```sql
ALTER TABLE Persons ADD PRIMARY KEY (PKey);
```

### IF ~ ELSE 조건문

```sql
SELECT IF(조건문, '참일때 값', '거짓일 때 값') FROM 테이블명
```

# 참고한 사이트

[https://www.w3schools.com/sql/sql_foreignkey.asp](https://www.w3schools.com/sql/sql_foreignkey.asp)
