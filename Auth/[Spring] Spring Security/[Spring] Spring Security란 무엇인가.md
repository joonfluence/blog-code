# 본론

### 패키지 설치

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'com.auth0:java-jwt:3.18.1'
}
```

### FilterChain

먼저 Spring Security를 알기 위해선, 서블릿을 알아야 한다. `서블릿(Servelet)`이란 자바로 구현된 웹 서버에 대한 구현 클래스를 말한하며, HttpServletRequest와 HttpServletResponse를 파라미터로 갖는다. Spring Security는 표준 서블릿 필터를 사용하기 때문에, 두 요소를 동일하게 갖는다. Spring Security는 추가로 필터를 내부적으로 구성하는데, 각 필터는 각자 역할이 있고 필터 사이의 종속성이 있으므로 순서가 중요하다. XML Tag를 이용한 네임스페이스 구성을 사용하는 경우 필터가 자동으로 구성되지만, 네임스페이스 구성이 지원하지않는 기능을 써야하거나 커스터마이징된 필터를 사용해야 할 경우 명시적으로 빈을 등록 할 수 있다.

클라이언트가 요청을 하면 `DelegatingFilterProxy`가 요청을 가로채고 Spring Security의 빈으로 전달합니다. `DelegatingFilterProxy`란 표준 서블릿 필터를 구현하고 있으며 내부에 위임대상(FilterChainProxy)를 갖고 있습니다. 이는 web.xml과 ApplicationContext 사이의 링크를 제공하는 역할을 합니다. DelegatingFilterProxy는 한 마디로 Spring의 애플리케이션 컨텍스트에서 얻은 Filter Bean을 대신 실행합니다.

### Spring Security의 구조

1. 사용자가 입력한 사용자 정보를 가지고 인증을 요청한다. (Request)
2. AuthenticationFilter가 이를 가로채 UsernamePasswordAuthenticationToken(인증용 객체)를 생성한다.
3. 필터는 요청을 처리하고 AuthenticationManager의 구현체 ProviderManager에 Authentication과 UsernamePasswordAuthenticationToken을 전달한다.
4. AuthenticationManager는 검증을 위해 AuthenticationProvider에게 Authentication과 UsernamePasswordAuthenticationToken을 전달한다.
5. 이제 DB에 담긴 사용자 인증정보와 비교하기 위해 UserDetailsService에 사용자 정보를 넘겨준다. DB에서 찾은 사용자 정보인 UserDetails 객체를 만든다.
6. DB에서 찾은 사용자 정보인 UserDetails 객체를 만든다.
7. AuthenticationProvider는 UserDetails를 넘겨받고 비교한다.
8. 인증이 완료되면 권한과 사용자 정보를 담은 Authentication 객체가 반환된다.
9. AuthenticationFilter까지 Authentication정보를 전달한다.
10. Authentication을 SecurityContext에 저장한다.

Authentication정보는 결국 SecurityContextHolder 세션 영역에 있는 SecurityContext에 Authentication 객체를 저장한다. 세션에 사용자 정보를 저장한다는 것은 전통적인 세션-쿠키 기반의 인증 방식을 사용한다는 것을 의미한다.

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

# 참고한 사이트

[https://velog.io/@shinmj1207/Spring-Spring-Security-JWT-%EB%A1%9C%EA%B7%B8%EC%9D%B8](https://velog.io/@shinmj1207/Spring-Spring-Security-JWT-%EB%A1%9C%EA%B7%B8%EC%9D%B8)
[https://gaemi606.tistory.com/entry/Spring-Boot-Spring-Security-JWT-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B3%BC%EC%A0%95](https://gaemi606.tistory.com/entry/Spring-Boot-Spring-Security-JWT-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B3%BC%EC%A0%95)
