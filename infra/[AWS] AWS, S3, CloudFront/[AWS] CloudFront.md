### 서론

CloudFront를 학습하기 위해선 Cache에 대해서 알아보아야 합니다. 그래서 먼저 Cache에 대해서 알아보도록 하겠습니다.

### 본론

캐시를 사용했을 떄의 장점은 네트워크를 통하지 않고도 이미 한 번 다운로드 받은 파일은 빠르게 다운로드 받을 수 있다.

Apache WebServer에서 `Open Config File` - `Open Conf File` 에서 아래와 같이 설정해줍니다.

```
SetEnv no-gzip 1
Header set Cache-Control "no-store"
```

캐시를 최신 상태로

```
Header set Cache-Control "max-age=31536000"
```

신선도의 문제가 발생된다.

성능이 좋으면서도 신선도가 높게 유지하는 것이 관건이다.
