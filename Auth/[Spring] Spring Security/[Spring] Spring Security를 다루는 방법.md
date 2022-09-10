# 본론

### 이론적 설명

먼저 Spring Security를 알기 위해선, 서블릿을 알아야 한다. `서블릿(Servelet)`이란 자바로 구현된 웹 서버에 대한 구현 클래스를 말한하며, HttpServletRequest와 HttpServletResponse를 파라미터로 갖는다. Spring Security는 표준 서블릿 필터를 사용하기 때문에, 두 요소를 동일하게 갖는다. Spring Security는 추가로 필터를 내부적으로 구성하는데, 각 필터는 각자 역할이 있고 필터 사이의 종속성이 있으므로 순서가 중요하다. XML Tag를 이용한 네임스페이스 구성을 사용하는 경우 필터가 자동으로 구성되지만, 네임스페이스 구성이 지원하지않는 기능을 써야하거나 커스터마이징된 필터를 사용해야 할 경우 명시적으로 빈을 등록 할 수 있다.

클라이언트가 요청을 하면 `DelegatingFilterProxy`가 요청을 가로채고 Spring Security의 빈으로 전달합니다. `DelegatingFilterProxy`란 표준 서블릿 필터를 구현하고 있으며 내부에 위임대상(FilterChainProxy)를 갖고 있습니다. 이는 web.xml과 ApplicationContext 사이의 링크를 제공하는 역할을 합니다. DelegatingFilterProxy는 한 마디로 Spring의 애플리케이션 컨텍스트에서 얻은 **Filter Bean**을 대신 실행합니다.

스프링 시큐리티는 SecurityContext에 인증된 Authentication 객체를 넣어두고 현재 스레드 내에서 공유되도록 관리하고 있는데요. SecurirtContext는 Interface로, **minimum security information associated with the current thread of execution**를 정의합니다. 인증 이후, 편의적으로 현재 인증된 세션유저를 가져오기 위해 @AuthenticationPrincipal 어노테이션을 통해 UserDetails 인터페이스를 구현한 유저 객체를 주입할 때 사용하는 편입니다.

- Session Stateless

API 서버는 유저의 세션을 관리하는 것이 아닌 특정 토큰에 의해서 요청 Request 헤더에 특정 토큰(주로 Access Token 이라 지칭)을 보내주면 인증이 완료되고 api 기능을 사용할 권한을 갖게 됩니다.

- Cors(Cross-Origin Resource Sharing) 교차 출처 자원 공유

보통 별도의 API 서버가 존재한다면 앱 어플리케이션인 SPA 프레임워크(react, vue, angular 등)를 사용하게 될텐데요. 이때 스프링 프로젝트와는 다른포트를 사용할 것입니다. 혹은 다른 서버(물리)일 수 있구요.

### 실제 구현하기

Security 설정을 위해 사용하던 WebSecurityConfigurerAdapter는 SpringSecurity 5.3 버젼부터 더 이상 쓰지 않음.

```java

// HttpSecurity 설정하기
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain httpConfigure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .cors().and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeHttpRequests((authz) -> authz.anyRequest().authenticated())
                .httpBasic(withDefaults());
        return http.build();
    }
}
```

# 참고한 사이트

[https://sas-study.tistory.com/356](https://sas-study.tistory.com/356)
[https://sas-study.tistory.com/357](https://sas-study.tistory.com/357)
[https://sas-study.tistory.com/359](https://sas-study.tistory.com/359)
[https://sas-study.tistory.com/360](https://sas-study.tistory.com/360)
[https://sas-study.tistory.com/362](https://sas-study.tistory.com/362)
[https://sas-study.tistory.com/363](https://sas-study.tistory.com/363)
[https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)
