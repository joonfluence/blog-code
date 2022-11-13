```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class CustomerControllerTestRestTemplateTest {

  @Autowired
  private TestRestTemplate testRestTemplate;

  @Test
  void shouldCreateNewCustomers() {

    HttpHeaders requestHeaders = new HttpHeaders();
    requestHeaders.add(HttpHeaders.CONTENT_TYPE, APPLICATION_JSON_VALUE);

    HttpEntity<String> requestEntity = new HttpEntity<>("""
      {
        "firstName": "Mike",
        "lastName": "Thomson",
        "id": 43
       }
      """,
      requestHeaders
    );

    ResponseEntity<Void> response = this.testRestTemplate
      .exchange("/api/customers", HttpMethod.POST, requestEntity, Void.class);

    assertThat(response.getStatusCodeValue())
      .isEqualTo(201);
  }
}
```
