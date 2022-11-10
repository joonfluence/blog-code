# 서론

# 본론

- 원격 접속

```
mysql -h [엔드포인트] -P 3306 -u [유저명] -p [데이터베이스명]
```

- mysql 동시 접속 확인

```sql
show variables like "%max_connection%";
```

- 최대 접속 가능 수 확인

```sql
show status like 'Max_used_connections';
```

- 변경하기

(아래 링크 참고)

# 참고한 사이트

[https://support.bespinglobal.com/ko/support/solutions/articles/73000524758--aws-rds-max-connections-%EC%88%98%EC%A0%95%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95](https://support.bespinglobal.com/ko/support/solutions/articles/73000524758--aws-rds-max-connections-%EC%88%98%EC%A0%95%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)
