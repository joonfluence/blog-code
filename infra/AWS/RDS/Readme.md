### 명령줄에서 IAM 인증을 사용하여 DB 클러스터에 연결하는 법 : AWS CLI 및 mysql 클라이언트

- IAM 인증 토크 생성

```
aws rds generate-db-auth-token \
   --hostname rdsmysql.123456789012.us-west-2.rds.amazonaws.com \
   --port 3306 \
   --region us-west-2 \
   --username jane_doe
```

- DB 클러스터에 연결

```
mysql --host=hostName --port=portNumber --ssl-ca=full_path_to_ssl_certificate --enable-cleartext-plugin --user=userName --password=authToken
```
