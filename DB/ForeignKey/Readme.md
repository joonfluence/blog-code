# 본론

### 설정 방법

- 외래키 추가하기 

```sql
ALTER TABLE 테이블명 ADD CONSTRAINT 포린키이름 FOREIGN KEY 자식속성 REFERENCES 부모테이블명(자식속성이 참고할 부모속성) ON DELETE CASCADE;
ALTER TABLE feed ADD CONSTRAINT FOREIGN KEY (user_id) REFERENCES user(id) ON DELETE CASCADE;
```

### 제약사항 

- ON DELETE CASCADE

`ON DELETE CASCADE` 옵션을 적용하면 부모 테이블에서 row 를 삭제할 경우 연결된 자식 테이블의 row 가 함께 삭제됩니다. 연결된 데이터를 한 번에 지울 수 있어 데이터의 관리가 편리해지고 일관성을 유지할 수 있습니다.

# 참고한 사이트 

[ON_DELETE_CASCADE](https://velog.io/@eensungkim/ON-DELETE-CASCADE-feat.-row-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A7%80%EC%9A%B0%EB%8A%94-%EB%B0%A9%EB%B2%95-TIL-78%EC%9D%BC%EC%B0%A8)