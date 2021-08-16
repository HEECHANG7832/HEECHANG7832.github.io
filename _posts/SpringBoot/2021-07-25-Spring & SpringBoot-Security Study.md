---
layout: post
title: "Security Study"
categories:
  - SpringBoot
tags:
  - SpringBoot
---



## 스프링 시큐리티

application.yml

security 사용자 추가

```
spring:
	security:
		user:
			name: user1
			password: 1111
			roles: USER
```



Auth 확인하기

```java
@RequestMapping("/auth")
public Authentication auth(){
    return SecurityContextHolder.getContext()
        .getAuthentication();
}
```



User가 접근/ Admin이 접근

```java
    @PreAuthorize("hasAnyAuthority('ROLE_USER')")
    @GetMapping("/user")
    public SecurityMessage user(@AuthenticationPrincipal UserDetails user){
        return SecurityMessage.builder()
                .user(user)
                .message("user page")
                .build();
    }

    @PreAuthorize("hasAnyAuthority('ROLE_ADMIN')")
    @GetMapping("/admin")
    public SecurityMessage admin(@AuthenticationPrincipal UserDetails user){
        return SecurityMessage.builder()
                .user(user)
                .message("admin page")
                .build();
    }
```



Config 설정 을 해야 위 페이지들 접근이 막히게 된다

```java
@EnableWebSecurity(debug = true) //Security 필터 체인을 확인 할 수 있다
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //사용자 추가
    @Bean
    UserDetailsService userService(){
        final PasswordEncoder pw = passwordEncoder();
        UserDetails user2 = User.builder()
                .username("user2")
                .password(pw.encode("1234"))
                .roles("USER").build();
        UserDetails admin = User.builder()
                .username("admin")
                .password(pw.encode("1234"))
                .roles("ADMIN").build();
        return new InMemoryUserDetailsManager(user2, admin);
    }

    //password encoder
    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    //security는 기본적으로 모두 막고 시작
    //permitAll로 풀어줘야 한다
    //필요한 security 필터를 추가하는 과정
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .formLogin()
                .and()
                .authorizeRequests(auth->{
                    auth.anyRequest().permitAll()
                            ;
                })
                ;

        http.andMatcher("/api/**"); //특정 리소스에 대해서만 구불할때
    }
}
```



인증 토큰 Authentication

- 특정 필터등을 통해 제공됨
- 이 토큰의 정보를 가지고 로그인, 권한, 그 외 정보를 확인 할 수 있다

- 사이트 내에서 통행증 역활



폼 로그인

UsernamePasswordAuthenticationFilter



### 3-1

basic-login

SecurityConfig

```java
    //관리자의 경우 유저의 권한을 모두 가지는 경우가 있기 때문
	@Bean
    RoleHierarchy roleHierarchy(){
        RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
        roleHierarchy.setHierarchy("ROLE_ADMIN > ROLE_USER");
        return roleHierarchy;
    }    

	@Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests(request->
                    request.antMatchers("/").permitAll() //기본적으로 모든 권한 허용
                            .anyRequest().authenticated() //요청에 대해선 인증이 필요
                )
                .formLogin(login->
                        login.loginPage("/login") //폼을 지정해서 기본 폼을 사용하지 않고 만들어진 페이지를 사용
                        .loginProcessingUrl("/loginprocess")
                        .permitAll() //로그인 페이지는 권한 허용해야됨
                        .defaultSuccessUrl("/", false) //로그인 이후 url, false면 로그인 하기 전 접속된 url을 바로 갈 수 있다
                        .authenticationDetailsSource(customAuthDetail) //커스텀한 AuthDetail을 추가해 줄 수 있다 auth에 detail내용 추가
                        .failureUrl("/login-error") //로그인 실패 했을때
                )
                .logout(logout->
                        logout.logoutSuccessUrl("/")) //로그아웃시 홈으로 이동
                .exceptionHandling(error->
                        error.accessDeniedPage("/access-denied") //exception 발생시 접근 불가 페이지로 전달
                )
                ;
    }


//CSS 같은 리소스는 Security에서 제외 한다
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring()
                .requestMatchers(
                        PathRequest.toStaticResources().atCommonLocations()
                )
        ;
    }
```

```java
@Component
public class CustomAuthDetail implements AuthenticationDetailsSource<HttpServletRequest, RequestInfo> {

    @Override
    public RequestInfo buildDetails(HttpServletRequest request) {
        return RequestInfo.builder()
                .loginTime(LocalDateTime.now())
                .remoteIp(request.getRemoteAddr())
                .sessionId(request.getSession().getId())
                .build();
    }

}
```



- thymeleaf 에서 security를 적용하는 태그

- 로그인 한 경우만 보여주고 싶을때

  ```html
  <div sec:authorize="isAuthenticated()">
    This content is only shown to authenticated users.
  </div>
  <div sec:authorize="hasRole('ROLE_ADMIN')">
    This content is only shown to administrators.
  </div>
  <div sec:authorize="hasRole('ROLE_USER')">
    This content is only shown to users.
  </div>
  ```



```java
//auth 정보 확인용 페이지
    @ResponseBody //JSON으로 내려주기 위해
    @GetMapping("/auth")
    public Authentication auth(){
        return SecurityContextHolder.getContext().getAuthentication();
    }

```



### 4-1

학생 권한에 대한 인증 토큰 만들기

Authentication을 구현한 Token

AuthenticationProvider을 구현한 Provider

보통은 UserDetails를 구현하는 정도지 인증처리를 하는 Provider까지 구현할 일은 드물다

```java
public class Student {

    private String id;
    private String username;
    private Set<GrantedAuthority> role;

}
public class StudentAuthenticationToken implements Authentication {

    private Student principal;
    private String credentials;
    private String details;
    private boolean authenticated;
    private Set<GrantedAuthority> authorities;


    @Override
    public String getName() {
        return principal == null ? "" : principal.getUsername();
    }

}

/*
SecurityConfig에 등록 해줘야하ㅏㅁ
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(studentManager);
        auth.authenticationProvider(teacherManager);
    }
*/
@Component
public class StudentManager implements AuthenticationProvider, InitializingBean {

    private HashMap<String, Student> studentDB = new HashMap<>();

    //전달 받은 Token의 아이디가 DB의 내용 아이디와 같으면 Auth을 만들어서 전달해준다
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        StudentAuthenticationToken token = (StudentAuthenticationToken) authentication;
        if(studentDB.containsKey(token.getCredentials())){
            Student student = studentDB.get(token.getCredentials());
            return StudentAuthenticationToken.builder()
                    .principal(student)
                    .details(student.getUsername())
                    .authenticated(true)
                    .authorities(student.getRole())
                    .build();
        }
        return null; //처리할수 없는 auth는 null로 넘겨야함
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return authentication == StudentAuthenticationToken.class;
    }

    //InitializingBean 의 구현, DB접근 대신 메모리에 객체로 구현 미리 Set을 채워둔다
    @Override
    public void afterPropertiesSet() throws Exception {
        Set.of(
                new Student("hong", "홍길동", Set.of(new SimpleGrantedAuthority("ROLE_STUDENT"))),
                new Student("kang", "강아지", Set.of(new SimpleGrantedAuthority("ROLE_STUDENT"))),
                new Student("rang", "호랑이", Set.of(new SimpleGrantedAuthority("ROLE_STUDENT")))
        ).forEach(s->
            studentDB.put(s.getId(), s)
        );
    }
}

```



학생 토큰으로 접근 가능한 Controller

```java
@Controller
@RequestMapping("/student")
public class StudentController {

    @PreAuthorize("hasAnyAuthority('ROLE_STUDENT')")
    @GetMapping("/main")
    public String main(){
        return "StudentMain";
    }

}
```



### 5

BasicAuthenticationFilter

- 기본 페이지를 사용할 수 없는 경우



기본 인증 Test class

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class BasicAuthenticationTest {

    @LocalServerPort
    int port;

    RestTemplate client = new RestTemplate();

    private String greetingUrl(){
        return "http://localhost:"+port+"/greeting";
    }

    @DisplayName("1. 인증 실패")
    @Test
    void test_1(){ //그냥 요청을 보내면 권한 없음 exception 발생
        HttpClientErrorException exception = assertThrows(HttpClientErrorException.class, () -> {
            client.getForObject(greetingUrl(), String.class);
        });
        assertEquals(401, exception.getRawStatusCode());
    }


    @DisplayName("2. 인증 성공")
    @Test
    void test_2() { //로그인을 가정한 request를 보낸 Test
        HttpHeaders headers = new HttpHeaders();
        headers.add(HttpHeaders.AUTHORIZATION, "Basic "+ Base64.getEncoder().encodeToString(
                "user1:1111".getBytes()
        ));
        HttpEntity entity = new HttpEntity(null, headers);
        ResponseEntity<String> resp = client.exchange(greetingUrl(), HttpMethod.GET, entity, String.class);
        assertEquals("hello", resp.getBody());
    }

    //TestTemplate을 사용하면 인증 Test를 쉽게 구현 가능
    @DisplayName("3. 인증성공2 ")
    @Test
    void test_3() {
        TestRestTemplate testClient = new TestRestTemplate("user1", "1111");
        String resp = testClient.getForObject(greetingUrl(), String.class);
        assertEquals("hello", resp);
    }

    //csrf 필터를 disable 해줘야함
    @DisplayName("4. POST 인증")
    @Test
    void test_4() {
        TestRestTemplate testClient = new TestRestTemplate("user1", "1111");
        ResponseEntity<String> resp = testClient.postForEntity(greetingUrl(), "jongwon", String.class);
        assertEquals("hello jongwon", resp.getBody());
    }


```



### 5 - 1

서로 다른 SecurityConfig를 사용하는 경우

MobileSecurityConfig.java

```java
@Order(1) //Config끼리의 순서 지정
@Configuration
public class MobSecurityConfig extends WebSecurityConfigurerAdapter {

    private final StudentManager studentManager;
    private final TeacherManager teacherManager;

    public MobSecurityConfig(StudentManager studentManager, TeacherManager teacherManager) {
        this.studentManager = studentManager;
        this.teacherManager = teacherManager;
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(studentManager);
        auth.authenticationProvider(teacherManager);
    }

    //BasicAuth filter 동작
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .antMatcher("/api/**")
                .csrf().disable()
                .authorizeRequests(request->request.anyRequest().authenticated())
                .httpBasic()
                ;
    }

}

```



Test작성

```java
    @DisplayName("1. choi:1 로 로그인해서 학생 리스트를 내려받는다.")
    @Test
    void test_1(){
        ResponseEntity<List<Student>> resp = testClient.exchange("http://localhost:" + port + "/api/teacher/students",
                HttpMethod.GET, null, new ParameterizedTypeReference<List<Student>>() {
                });

        assertNotNull(resp.getBody());
        assertEquals(3, resp.getBody().size());
    }

```





### 6

실제 DB에 User 객체를 넣고 가져오면서 인증하는 방법

UserDetails 구현

UserDetailsService구현

SecurityConfig에 등록해서 사용

```java
@Table(name="sp_user_authority")
@IdClass(SpAuthority.class)
public class SpAuthority implements GrantedAuthority {

    @Id
    @Column(name="user_id")
    private Long userId;

    @Id
    private String authority;


}


@Table(name="sp_user")
public class SpUser implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;

    private String email;

    private String password;

    @OneToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
    @JoinColumn(name = "user_id", foreignKey = @ForeignKey(name="user_id"))
    private Set<SpAuthority> authorities;

    private boolean enabled;

    @Override
    public String getUsername() {
        return email;
    }

    @Override
    public boolean isAccountNonExpired() {
        return enabled;
    }

    @Override
    public boolean isAccountNonLocked() {
        return enabled;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return enabled;
    }

}

@Service
@Transactional
public class SpUserService implements UserDetailsService {

    @Autowired
    private SpUserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return userRepository.findUserByEmail(username).orElseThrow(
                ()->new UsernameNotFoundException(username));
    }

    public Optional<SpUser> findUser(String email) {
        return userRepository.findUserByEmail(email);
    }

    public SpUser save(SpUser user) {
        return userRepository.save(user);
    }

    public void addAuthority(Long userId, String authority){
        userRepository.findById(userId).ifPresent(user->{
            SpAuthority newRole = new SpAuthority(user.getUserId(), authority);
            if(user.getAuthorities() == null){
                HashSet<SpAuthority> authorities = new HashSet<>();
                authorities.add(newRole);
                user.setAuthorities(authorities);
                save(user);
            }else if(!user.getAuthorities().contains(newRole)){
                HashSet<SpAuthority> authorities = new HashSet<>();
                authorities.addAll(user.getAuthorities());
                authorities.add(newRole);
                user.setAuthorities(authorities);
                save(user);
            }
        });
    }

    public void removeAuthority(Long userId, String authority){
        userRepository.findById(userId).ifPresent(user->{
            if(user.getAuthorities()==null) return;
            SpAuthority targetRole = new SpAuthority(user.getUserId(), authority);
            if(user.getAuthorities().contains(targetRole)){
                user.setAuthorities(
                        user.getAuthorities().stream().filter(auth->!auth.equals(targetRole))
                                .collect(Collectors.toSet())
                );
                save(user);
            }
        });
    }
}

//SecurityConfig에 UserDetailsService를 등록
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(spUserService);
    }
```
