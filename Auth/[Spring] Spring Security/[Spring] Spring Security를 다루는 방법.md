# 본론

### 이론적 설명

먼저 Spring Security를 알기 위해선, 서블릿을 알아야 한다. `서블릿(Servelet)`이란 자바로 구현된 웹 서버에 대한 구현 클래스를 말한하며, HttpServletRequest와 HttpServletResponse를 파라미터로 갖는다. Spring Security는 표준 서블릿 필터를 사용하기 때문에, 두 요소를 동일하게 갖는다. Spring Security는 추가로 필터를 내부적으로 구성하는데, 각 필터는 각자 역할이 있고 필터 사이의 종속성이 있으므로 순서가 중요하다. XML Tag를 이용한 네임스페이스 구성을 사용하는 경우 필터가 자동으로 구성되지만, 네임스페이스 구성이 지원하지않는 기능을 써야하거나 커스터마이징된 필터를 사용해야 할 경우 명시적으로 빈을 등록 할 수 있다.

클라이언트가 요청을 하면 `DelegatingFilterProxy`가 요청을 가로채고 Spring Security의 빈으로 전달합니다. `DelegatingFilterProxy`란 표준 서블릿 필터를 구현하고 있으며 내부에 위임대상(FilterChainProxy)를 갖고 있습니다. 이는 web.xml과 ApplicationContext 사이의 링크를 제공하는 역할을 합니다. DelegatingFilterProxy는 한 마디로 Spring의 애플리케이션 컨텍스트에서 얻은 **Filter Bean**을 대신 실행합니다.

스프링 시큐리티는 SecurityContext에 인증된 Authentication 객체를 넣어두고 현재 스레드 내에서 공유되도록 관리하고 있는데요. SecurirtContext는 Interface로, **minimum security information associated with the current thread of execution**를 정의합니다. 인증 이후, 편의적으로 현재 인증된 세션유저를 가져오기 위해 @AuthenticationPrincipal 어노테이션을 통해 UserDetails 인터페이스를 구현한 유저 객체를 주입할 때 사용하는 편입니다.

- Session Stateless

API 서버는 유저의 세션을 관리하는 것이 아닌 특정 토큰에 의해서 요청 Request 헤더에 특정 토큰(주로 Access Token 이라 지칭)을 보내주면 인증이 완료되고 api 기능을 사용할 권한을 갖게 됩니다.

- Cors(Cross-Origin Resource Sharing) 교차 출처 자원 공유

보통 별도의 API 서버가 존재한다면 앱 어플리케이션인 SPA 프레임워크(react, vue, angular 등)를 사용하게 될텐데요. 이때 스프링 프로젝트와는 다른포트를 사용할 것입니다. 혹은 다른 서버(물리)일 수 있구요.

### HttpSecurity 설정하기

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

### UserDetails, UserDetailsService 인터페이스 구현하기

- UserDetails : 우리가 구현해야 할 User 객체를 말합니다.

```java
public class SecurityUser implements UserDetails {
    private Member member;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return Collections.emptyList();
    }

    @Override
    public String getPassword() {
        return member.getPassword();
    }

    @Override
    public String getUsername() {
        return member.getName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}

```

- UserDetailsService : UserDetailsService에서 UserDetails을 구현하여, loadUserByUsername라는 오버라이딩 메소드의 Request에서 받은 로그인 데이터를 활용하여 로그인 로직을 처리해줍니다. 이때 해당 메소드가 인증된 결과를 가지고 UserDetails 인터페이스를 구현하는 인증대상객체를 리턴해줍니다.

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String loginId) throws UsernameNotFoundException {
        System.out.println("인증을 받습니다.");
        //로그인 로직 시작
        // loginId를 이용하여 DB에서 User 객체를 가져옵니다.
        // User user = mapper.getUser(loginID);
        // User의 정보를 SecurityUser 에 담아줍니다. 이는 생성자를 이용하는 편입니다.
        return new SecurityUser();
    }
}
```

실제로는 이 부분에서 Mapper라던지 Repository라던지 DI가 발생하여 의존성을 주입해주고 디비로 다녀와서 id에 맞는 데이터를 가져와서 Security 객체에 담아주는 작업을 해줍니다.

### 인증 Filter 작성하기

앞서 인증 대상 객체(UserDetails)와 인증 서비스 로직(UserDetailsService)를 구현했으니, 
REST API 기반의 서버를 구현할 것이므로 매 요청마다 인증 처리를 통과해야 합니다. 그렇게 되면 인증 토큰이 발행되어야 하지만, 우선 무조건 인증되는 방식으로 먼저 구현해보겠습니다.

```java
public class OncePerRequestFilterImpl extends OncePerRequestFilter {
    @Autowired
    private UserDetailsServiceImpl userDetailsServiceImpl;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        UserDetails authentication = userDetailsServiceImpl.loadUserByUsername("sample");
        UsernamePasswordAuthenticationToken auth = new UsernamePasswordAuthenticationToken(authentication.getUsername(), null, null);
        SecurityContextHolder.getContext().setAuthentication(auth);
        filterChain.doFilter(request, response);
    }
}
```

OncePerRequestFilter를 상속받아 doFilterInternal 메소드를 재정의해줍니다. 매 Request 마다 Controller 전에 해당 필터를 수행할 것이고 SecurityContextHolder에 있는 Context 객체의 authentication 여부에 따라 인증여부가 결정되게 됩니다. 

### AuthenticationEntryPoint 작성하기

이번엔 인증작업을 처리하기 위한 로직 구현을 살펴보겠습니다. 

### JWT 인증 토큰 

SecurityContext에 어떤 방식으로 인증 정보를 저장할 수 있을까요? 
불러오는 방법은 토큰의 Header 값을 불러옵니다. 
저장해야 할 정보는 UsernamePasswordAuthenticationToken 입니다.
저장 방법은 SecurityContextHolder.setContext를 사용합니다. 

### Header의 토큰 정보 불러오기 

# 참고한 사이트

[https://sas-study.tistory.com/356](https://sas-study.tistory.com/356)
[https://sas-study.tistory.com/357](https://sas-study.tistory.com/357)
[https://sas-study.tistory.com/359](https://sas-study.tistory.com/359)
[https://sas-study.tistory.com/360](https://sas-study.tistory.com/360)
[https://sas-study.tistory.com/362](https://sas-study.tistory.com/362)
[https://sas-study.tistory.com/363](https://sas-study.tistory.com/363)
[https://devbksheen.tistory.com/entry/Spring-Security-JWT](https://devbksheen.tistory.com/entry/Spring-Security-JWT)
[https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)
[https://velog.io/@gmtmoney2357/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0-Authentication-SecurityContext](https://velog.io/@gmtmoney2357/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0-Authentication-SecurityContext)