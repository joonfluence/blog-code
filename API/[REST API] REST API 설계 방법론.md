# 서론

오늘은 REST API 설계 방법론에 관하여 알아보도록 하겠습니다.

# 본론

## REST API Maturity Model (REST API 성숙도 모델)

### Level 0

Level 0은 웹 매커니즘을 사용하지 않고 HTTP를 원격 호출을 위한 전송 시스템으로 사용하는 경우이다. RPC(Remote Procedure Call)처럼 리소스 구분 없이 설계된 HTTP API이다.
하나의 End-point를 사용해서 HTTP Method도 반드시 POST가 된다. 그래서 서로 다른 매개변수를 통해서만 여러 동작을 하게 된다.

```javascript
// POST /api/user, Request
{
  "function": "getUser",
  "arguments" [
    "1"
  ]
}
```

```javascript
// HTTP/1.1 200 OK
{
  "result" {
    "id": "1"
    "name": "honey",
  }
}
```

```javascript
CREATE: POST / api / user;
READ: POST / api / user;
UPDATE: POST / api / user;
DELETE: POST / api / user;
```

### Level 1

Level 1은 리소스 개념을 도입한다. 모든 요청을 하나의 End-point로 보내는 것이 아니라 개별 리소스와 통신하게 된다.
HTTP Method는 GET과 POST만 사용하고 StatusCode는 무조건 200으로 전달한다. 헤더에 Content-Type이나 Cache 관련 정보도 제공하지 않는다.

```javascript
// POST /api/users/create
{
  "name": "honey"
}
```

```javascript
// HTTP/1.1 200 OK
{
  "result" {
    "error": "already exist member"
  }
}
```

```javascript
CREATE: POST / api / users / create;
READ: GET / api / users / 1;
UPDATE: POST / api / users / update;
DELETE: POST / api / users / remove / 1;
```

### Level 2

Level 2는 4가지 HTTP Method를 사용해서 CRUD를 표현하고 StatusCode도 활용하여 반환한다.
URI에는 행위(Action)가 포함되지 않고 HTTP Method로 표현한다. GET은 매번 같은 결과를 반환하고, 헤더에 Content-Type을 제공하고 멱등성을 보장하는 GET의 경우 캐시가 적용된다.
현재 가장 많은 REST API가 이 단계에 해당한다고 한다.

```javascript
// POST /api/users
{
  "name": "honey"
}
```

```javascript
// HTTP/1.1 201 Created
// Content-Type: application/json
{
  "result" {
    "id": "1",
    "name": "honey"
  }
}
```

```javascript
CREATE: POST / api / users;
READ: GET / api / users / 1;
UPDATE: PUT / api / users / 1;
DELETE: DELETE / api / users / 1;
```

### 3단계

마지막 단계인 Level 3이다. API 서비스의 모든 End-point를 최초 진입점이 되는 URI를 통해 Hypertext Link 형태로 제공한다.
추가적으로 다음 Request에 필요한 End-point까지 제공을 한다. 이는 Uniform Interface의 HATEOAS를 의미한다.

# 참고한 사이트

[https://jaehoney.tistory.com/176](https://jaehoney.tistory.com/176)
