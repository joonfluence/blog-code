### 스냅샷이란

전체 DB 인스턴스를 백업하여 DB 인스턴스의 스토리지 볼륨 스냅샷을 생성한다.

```
aws rds create-db-snapshot \
    --db-instance-identifier mynewdbinstance \
    --db-snapshot-identifier mydbsnapshot
```

### AWS RDS에서 SnapShot을 어떻게 불러오지?

```
aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier mynewdbinstance \
    --db-snapshot-identifier mydbsnapshot
```

### ReadOnly RDS 서버란?

- 어떻게 ReadOnly로만 가능하도록 처리할 수 있을까?

### RDS 유저 구분 하는 방법은?

- 모든 동작을 처리할 수 있는 root 계정과 CRUD만 가능한 user 계정을 구분하는 방법은 무엇일까?

# 참고한 사이트

[https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html)
