# 본문

1. application.properties 파일 yml로 변경하기
2. application.yml을 보면 위와 같이 맨 위에 공통으로 쓰이는 설정 값들을 놓고, ---을 구분 후에 아래의 prod, dev, local 환경을 분리한다.

```
java -jar -Dspring.profiles.active=prod *.jar
```

# 참고한 사이트

[https://wildeveloperetrain.tistory.com/8](https://wildeveloperetrain.tistory.com/8)
[https://devlog-wjdrbs96.tistory.com/343](https://devlog-wjdrbs96.tistory.com/343)