# 조회

## 비밀번호 변경

```sql
set PASSWORD FOR 'root'@'localhost' = PASSWORD("1234");
```

## 기본

- 전체 읽기 : SELECT \* from `테이블명` : 테이블의 전체 내용을 선택하여 데이터를 읽어옴.
- 해당 열 읽기 : SELECT `열 이름` from topic : 해당 열의 데이터를 읽어옴.
- 내림/오름차순 정렬 : SELECT `칼럼명` from topic `where` author order by id desc/asc : 아이디 순서대로 값을 오름차순으로 읽어온다.
  - 복수의 필드 정렬 : SELECT `칼럼명` ORDER BY `NAME` ASC, `DATETIME` DESC;
- 기타 조건
  - 범위 제한 : ex) BETWEEN 8 AND 9
  - 여러 항목 추출 : IN (‘France, Italy’)
  - 갯수 조회 : COUNT(`칼럼명`)
  - 시간 : HOUR(시간칼럼명), MONTH

## 조건문

- 조건 추가
  - WHERE : SELECT `칼럼명` from `테이블` where `칼럼`='특징' : column 명이 `특징`인 데이터를 읽음.
    - ex) SELECT \* from Class where grade=’A’
    - 가장 오랜 날짜 찾기 : SELECT NAME FROM ANIMAL_INS WHERE DATETIME = (SELECT MIN(DATETIME) FROM ANIMAL_INS);
    - 여러 값 중 찾기 : WHERE NAME in ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
  - IF : SELECT animal_type, if(`조건`, `참일 때 값`, `거짓일 때 값`), sex_upon_intake from animal_ins order by animal_id;
- 숫자 세기 : SELECT `COUNT(*)` AS count FROM ANIMAL_INS;
- 중복/NULL 제거 조회 : SELECT `COUNT(DISTINCT NAME)` FROM ANIMAL_INS WHERE NOT NAME IS `NULL`;

## 그룹연산

- GROUP BY : SELECT `ANIMAL_TYPE`, `COUNT(ANIMAL_TYPE)` as count FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE ASC;
  - 조건 추가 : SELECT `ANIMAL_TYPE`, `COUNT(ANIMAL_TYPE)` as count FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE ASC having INTAGE_CONDITION='Normal';
- 추출
  - HAVING : SELECT grade, COUNT(\*_) AS students FROM test GROUP BY grade HAVING COUNT(_) > 1\*

## 필터링

### 테이블 간 결합

- JOIN : 조인문 사용해서 두 테이블 간 데이터를 연관 지어 가져올 수 있다.

### 재귀

```sql
with recursive time as (
    select 0 as h
    union all
    select h+1 from time where h < 23
)
select h, count(datetime)
from time
    left join animal_outs on h = hour(datetime)
group by h
order by h;
```

## 요약

1. GROUP BY는 주로 COUNT, SUM, AVERAGE 등 값 계산에 주로 활용됨. 테이블을 분해하는 역할을 함.
2. JOIN은 데이터의 공통속성을 바탕으로 두 테이블 값의 교집합 결과를 호출해주기 위해 사용한다. JOIN은 INNER JOIN의 축약어이다.
3. INNER JOIN과 OUTER JOIN의 차이점은 무엇인가? The major difference between inner and outer joins is that **inner joins result in the intersection of two tables**, whereas outer joins result in the union of two tables.
4. Left Join은 left 테이블의 값을 전부 가져온다. Right 테이블은 left 테이블과 일치하는 경우에만 가져온다.

# 생성

### 테이블 칼럼 생성

```sql
ALTER TABLE `employee` ADD `comments` VARCHAR(200) NOT NULL;

ALTER TABLE {TABLENAME}
ADD {COLUMNNAME} {TYPE} {NULL|NOT NULL}
CONSTRAINT {CONSTRAINT_NAME} DEFAULT {DEFAULT_VALUE}
WITH VALUES
```

### 테이블 생성

```sql
Create Table CustomerProductList(
  CustomerCorpId int(2) not null,
  ProductId int(2) not null,
  IsContain boolean not null,
  FOREIGN KEY(CustomerCorpId) references CustomerCorp(PKey),
  FOREIGN KEY(ProductId) references Product(PKey)
);
```
