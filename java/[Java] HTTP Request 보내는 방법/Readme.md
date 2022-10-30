# 본론

오늘은 Java 11 HttpClient (자바11 HttpClient) 기능을 살펴보겠습니다.

```java
package com.example.demo.util;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.Map;

public class HttpClientJava11 {
    public static void post(String address, Map<?, ?> params, String[] headers) throws Exception {
        ObjectMapper objectMapper = new ObjectMapper();
        // params 요소들을 Body로 담는다.
        String requestBody = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(params);

        // Sting -> BodyPublisher로 형식을 변환한다.
        HttpRequest.BodyPublisher body = HttpRequest.BodyPublishers.ofString(requestBody);  //post 파라미터 최종 모습 입니다.

        // HTTP Connection을 연결한다.
        HttpClient client = HttpClient.newBuilder().version(HttpClient.Version.HTTP_1_1).build();

        // String -> URI로 형식을 변환한다.
        URI url = new URI(address);

        // HTTP 요청을 보낸다.
        String result = client.sendAsync(HttpRequest.newBuilder(url).POST(body).headers(headers).build(), HttpResponse.BodyHandlers.ofString()).thenApply(HttpResponse::body).get();

        // 응답을 찍어본다.
        System.out.println(result);
    }
}

```

# 참고한 사이트

[https://lts0606.tistory.com/455](https://lts0606.tistory.com/455)
