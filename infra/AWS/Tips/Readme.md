# 서론

오늘은 AWS 관리를 하면서, 기억해두면 유용한 팁들을 정리해보겠습니다.

# 본론

**계정 관리**

1. 루트 계정을 생성한다.
2. 그 아래 하위 계정을 생성한다.
3. 보안을 위해, MFA(Multi-Factor Authentication)을 설정해주는 것이 좋다.

**관리 순서**

1. 서비스 위치와 가까운 리젼을 선택한다.
2. 해당 리젼의 instance를 생성하고 관리한다. IAM 콘솔에서 여러 정보들을 확인해볼 수 있다.
3. 항상 다른 리젼의 instance가 활성화되어 있는지 파악할 필요가 있다.

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
