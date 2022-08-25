# 본론

### 설치

```groovy
dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-web'
        implementation 'org.springframework.boot:spring-boot-starter-security'
        implementation 'com.auth0:java-jwt:3.18.1'
}
```

시큐리티 설정을 해준다. 일단 configure()는 기본만..
@EnableWebSecurity로 시큐리티 사용을 설정하면, 자동으로 스프링 시큐리티에서 몇 가지 URL을 생성한다.

```java
@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .cors().and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .anyRequest().authenticated();
    }
}
```

UsernamePasswordAuthenticationFilter를 상속받아 로그인, 로그인 실패, 로그인 성공 처리를 한다.
1. 사용자가 아이디(username)과 비밀번호를 입력하면, 
2. UsernamePasswordAuthenticationFilter는 UsernamePasswordAuthenticationToken을 생성한 후 AuthenticationManager로 전달한다.
3. 그 후 해당 유저에 대한 검증은 AuthenticationManager가 처리하게 된다.

```java
@Slf4j
public class CustomAuthenticationFilter extends UsernamePasswordAuthenticationFilter {
    /* 로그인 경로로 요청이 왔을 때 호출되는 메서드 */
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // username과 password를 이용해 Authentication 타입의 토큰 생성
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(username, password);
        return authenticationManager.authenticate(authenticationToken);
    }
}
```

만약 username과 password외에 다른 값을 입력받고 싶다면 request.getParameter()를 이용할 수 있다.
검증 후 성공, 실패 처리는 successfulAuthentication()과 unsuccessfulAuthentication()를 통해 한다.

```java
/* 로그인에 성공했을 때 호출되는 메서드
* accessToken, refreshToken 응답 값에 세팅
* */
@Override
protected void successfulAuthentication(HttpServletRequest request,
                                        HttpServletResponse response,
                                        FilterChain chain,
                                        Authentication authentication) throws IOException {
    // 로그인에 성공한 유저
    final String username = (String) authentication.getPrincipal();

    // response body에 넣을 값 생성
        final Tokens tokens = jwtProvider.getTokens(username, authentication);
    Map<String, Object> body = new LinkedHashMap<>();
    body.put("access_token", tokens.getAccessToken());
    body.put("refresh_token", tokens.getRefreshToken());

    response.setStatus(HttpStatus.OK.value());
    response.setContentType(MediaType.APPLICATION_JSON_VALUE);

    new ObjectMapper().writeValue(response.getOutputStream(), body);
}

/* 로그인 실패 시 호출되는 메서드
* AuthenticationService 에서 발생하는 exception handling
* */
@Override
protected void unsuccessfulAuthentication(HttpServletRequest request, HttpServletResponse response, AuthenticationException failed) throws IOException {
    log.error("unsuccessfulAuthentication failed.getLocalizedMessage(): {}", failed.getLocalizedMessage());

    response.setStatus(HttpStatus.UNAUTHORIZED.value());
    response.setContentType(MediaType.APPLICATION_JSON_VALUE);

    Map<String, Object> body = new LinkedHashMap<>();
    body.put("code", HttpStatus.UNAUTHORIZED.value());
    body.put("error", failed.getMessage());

    new ObjectMapper().writeValue(response.getOutputStream(), body);
}
```

이제 위에서 작성한 CustomAuthenticationFilter를 등록해준다.

```java
@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
     @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .cors().and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .addFilter(new CustomAuthenticationFilter());
    }
}
```

### 토큰검증

이제 http header에 토큰을 보내면, 서버에서 읽고 유효한 토큰인지 판단하는 과정이 필요하다.
이 과정은 OncePerRequestFilter를 구현해 만들 수 있다.

OncePerRequestFilter는 http 요청 당 한 번만 실행되도록 보장되는 필터다.
요청마다 토큰을 검증해야 하므로 해당 필터를 사용해 검증할 수 있다.

```java
public class CustomAuthorizationFilter extends OncePerRequestFilter {
private final String TOKEN_PREFIX = "Bearer";
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        if (request.getServletPath().equals("/login")) {    // 로그인은 그냥 건너 뛴다
            filterChain.doFilter(request, response);
        } else {    // 로그인 외 모든 요청에는 filter 처리 한다

            //1. 요청 헤더에서 인증 값을 가져온다.
            String authorizationHeader = request.getHeader(HttpHeaders.AUTHORIZATION);

            //2. 인증 토큰이 존재하고, 그 값이 Bearer 토큰이면, 토큰을 decode 한다.
            if (authorizationHeader != null && authorizationHeader.startsWith(TOKEN_PREFIX)) {
                String token = authorizationHeader.substring(TOKEN_PREFIX.length());
                try {
                    // 토큰 유효성 검사 생략

                    //3. 토큰이 유효하다면 securityContextHolder에 인증 값을 세팅한다.
                    SecurityContextHolder.getContext().setAuthentication(authenticationToken);
                    filterChain.doFilter(request, response);
                } catch (BadCredentialsException | JWTVerificationException e) {
                    //2-1. 토큰 decode에 실패했다면, 에러 메시지를 응답한다.
                    log.error("Fail Decode Authorization Token");

                    response.setStatus(HttpStatus.UNAUTHORIZED.value());
                    response.setContentType(MediaType.APPLICATION_JSON_VALUE);

                    Map<String, Object> body = new LinkedHashMap<>();
                    body.put("code", HttpStatus.UNAUTHORIZED.value());
                    body.put("error", e.getMessage());

                    new ObjectMapper().writeValue(response.getOutputStream(), body);
                }
            } else {
                filterChain.doFilter(request, response);
            }
        }
    }
}
```

그리고 나서 필터를 등록해준다. addFilterBefore()는 앞의 인자값이 뒤의 필터 클래스보다 먼저 실행되는 필터로 등록하겠다는 뜻이다.

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http.csrf().disable()
            .cors().and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .addFilter(new CustomAuthenticationFilter())
            .addFilterBefore(new CustomAuthorizationFilter(), UsernamePasswordAuthenticationFilter.class);
}
```
### 

[https://velog.io/@shinmj1207/Spring-Spring-Security-JWT-%EB%A1%9C%EA%B7%B8%EC%9D%B8](https://velog.io/@shinmj1207/Spring-Spring-Security-JWT-%EB%A1%9C%EA%B7%B8%EC%9D%B8)
[https://gaemi606.tistory.com/entry/Spring-Boot-Spring-Security-JWT-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B3%BC%EC%A0%95](https://gaemi606.tistory.com/entry/Spring-Boot-Spring-Security-JWT-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B3%BC%EC%A0%95)