# 서론

오늘은 Spring Boot 환경에서 간단한 REST API를 개발하는 방법에 관해서 알아보도록 하겠습니다.

# 본론

### 학습할 어노테이션

어노테이션이란? 일종의 함수로, 스프링에 해당 기능을 추가하는 역할을 대신한다.

- @RestController? @ResponseBody 어노테이션을 붙이지 않아도 된다.
- @Controller? View 페이지를 찾아서 띄워주는 역할을 한다.

- @RequestMapping? HTTP method 별 요청을 유동적으로 처리하는 어노테이션.

ex) @RequestMapping(method = RequestMethod.GET, path="/")과 @GetMapping("/")은 서로 같은 형태.

- @GetMapping? 특정 경로로 GET 요청이 왔을 때, 처리해준다.
- @PostMapping? 특정 경로로 POST 요청이 왔을 때, 처리해준다.
- @DeleteMapping? 특정 경로로 DELETE 요청이 왔을 때, 처리해준다.
- @PutMapping? 특정 경로로 PUT 요청이 왔을 때, 처리해준다.

- @PathVariable? 아래와 같이, path로 전달된 변수를 처리해줌.

```java
@GetMapping("/users/{id}")
public UserProfile getUserprofile(@PathVariable("id") String id){
    return userMap.get(id);
}
```

- @RequestBody? HttpMessageConverter를 거쳐, HTTP 요청 본문에 담긴 값들을 자바 객체로 변환시키고 객체에 저장한다.
- @ResponseBody? 반환 값을 객체로 전달받을 수 있다.

```java
@GetMapping("hello-string")
@ResponseBody
public String helloString(@RequestParam("name") String name) {
    return "hello " + name;
}
```

- @RequestParam? query로 전달되는 parameter

```java
// PUT, /users/3?name=홍길철&phone=111-1115&address=서울시 강남구 대치5
@PutMapping("/users/{id}")
public UserProfile updateUserProfile(@PathVariable("id") String id, @RequestParam("name") String name, @RequestParam("phone") String phone, @RequestParam("address") String address){
    UserProfile userProfile = userMap.get(id);
    System.out.println("userProfile = " + userProfile);
    userProfile.setName(name);
    userProfile.setPhone(phone);
    userProfile.setAddress(address);
    return userMap.get(id);
}
```
