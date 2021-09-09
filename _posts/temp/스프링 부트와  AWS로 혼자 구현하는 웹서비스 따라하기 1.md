

# 스프링 부트와  AWS로 혼자 구현하는 웹서비스 따라하기 1



**그레이들 프로젝트를 스프링부트 프로젝트로 변경하기**

**build.gradle**

```
buildscript {
    ext { //build.gradle 에서 사용하는 전역변수를 설정
        springBootVersion = '2.1.9.RELEASE'
    }
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

//플러그인 의존성 적용, 자바와 스프링부트를 사용하기 위해 필수 플러그인 4개
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management' //스프링 부트의 의존성을 관리해주는 플러그인

group 'com.jojoldu.book'
version '1.0.4-SNAPSHOT-'+new Date().format("yyyyMMddHHmmss")
sourceCompatibility = 1.8   

//각종 의존성들을 어떤 원격 저장소에서 받을지
repositories {
    mavenCentral() //업로드하기 힘듬
    jcenter()
}

//프로젝트 개발에 필요한 의존성들을 선언
dependencies {
    compile('org.springframework.boot:spring-boot-starter-web') //
    compile('org.projectlombok:lombok') //롬복
    compile('org.springframework.boot:spring-boot-starter-data-jpa') //Spring Data JPA
    compile('com.h2database:h2') //인메모리 관계형 데이터베이스, 테스트 용도로 사용, 프로젝트 재 시작시 자동으로 초기화
        
    compile('org.springframework.boot:spring-boot-starter-mustache') //mustache 의존성 등록


    compile('org.springframework.boot:spring-boot-starter-oauth2-client')
    compile('org.springframework.session:spring-session-jdbc')

    compile("org.mariadb.jdbc:mariadb-java-client")

    testCompile('org.springframework.boot:spring-boot-starter-test') //
    testCompile("org.springframework.security:spring-security-test")
}


```



Enable Auto-import //  그래이들 옵션 변경시 자동 반영



**테스트 코드 작성 원리**

TDD : 테스트가 주도하는 개발 즉, 테스트  코드를 먼저 작성하는 것

1. 항상 실패하는 테스트를 먼저 작성
2. 테스트가 통과하는 프로덕션 코드를 작성
3. 테스트가 통과하면 프로덕션 코드를 리팩토링

단위 테스트 : 

- 개발 단계 초기에 문제를 발견하게 도와준다
- 리팩토링, 라이브러리 업그레이드 상황에 기존 코드가 정상동작하는지 확인가능
- 단위 테스트 자체가 문서의 역활

톰캣을 올렸다 내리는 수고로움, 눈으로 확인해야되는 휴먼에러 가능성



**테스트를 도와주는 프레임워크**

JUnit - Java      //    CppUnit - C++



패키지 생성 >> 자바 클래스 생성



**Application.java**

```java
@SpringBootApplication //스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성, 항상 프로젝트 최상단에 위치
public class Application {
 public static void main(String[] args) {
        SpringApplication.run(Application.class, args); //내장 Web Application Server 실행, 스프링부트로 만들어진 java 실행파일인 jar로 실행
    }
}
```

내장 WAS를 사용하는 이유 - 언제 어디서나 같은 환경에서 스프링부트를 배포

외장WAS의 종류와 버전, 설정을 일치시켜야 함



**HelloController.java**

```java
@RestController //컨트롤러를 JSON을 반환하는 컨트롤러로 만듬
//@ResponseBody
public class HelloController {

    @GetMapping("/hello") //Get 요청을 받을 수 있는 API를 만든다
    //@RequestMapping(method = RequestMethod.GET)
    public String hello() {
        return "hello";
    }

    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam("name") String name,
                                     //외부에서 API로 넘긴 파라미터를 가져오는 어노테이션
                                     @RequestParam("amount") int amount) {
        return new HelloResponseDto(name, amount);
    }

}
```



**HelloControllerTest.java**

```java
@RunWith(SpringRunner.class) //JUnit에 내장된 실행자 외에 다른 샐행자를 실행
//스프링 부트 테스트와 JUnit사이의 연결
@WebMvcTest(controllers = HelloController.class,
        excludeFilters = {
        @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = SecurityConfig.class)
        }
) //선언할 경우 @Controller, @ControllerAdvice 등 사용가능
public class HelloControllerTest {

    @Autowired //Bean 주입
    private MockMvc mvc; //스프링 MVC 테스트의 시작, 웹 API를 테스트할때 사용, 이 클래스를 통해 GET, POST등에 대한 API 테스트 진행

    @WithMockUser(roles="USER")
    @Test
    public void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello")) // /hello GET요청
                .andExpect(status().isOk()) // HTTP header Status 검증 200, 404등
                .andExpect(content().string(hello)); //응답이 hello인지??
    }

    @WithMockUser(roles="USER")
    @Test
    public void helloDto가_리턴된다() throws Exception {
        String name = "hello";
        int amount = 1000;

        mvc.perform(
                    get("/hello/dto")
                            .param("name", name) //API 테스트할때 요청 파라미터를 설정
                            .param("amount", String.valueOf(amount))) //String만 허용됨
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(name))) //JSON응답값을 필드별로 검증
                .andExpect(jsonPath("$.amount", is(amount)));
    }
}
```

**롬복**

자바 개발시 자주 사용하는 Getter, Setter, 기본 생성자, toString등을 어노테이션으로 자동 생성해 줌



**HelloResponseDto.java**

```java
@Getter //선언된 모든 필드의 get 메소드를 생성
@RequiredArgsConstructor //선언된 모든 final 필드가 포함된 생성자를 생성
public class HelloResponseDto {

    private final String name;
    private final int amount;

}
```



**HelloResponseDtoTest.java**

```java
package com.jojoldu.book.springboot.web.dto;

import org.junit.Test;

import static org.assertj.core.api.Assertions.assertThat;

public class HelloResponseDtoTest {

    @Test
    public void 롬복_기능_테스트() {
        //given
        String name = "test";
        int amount = 1000;

        //when
        HelloResponseDto dto = new HelloResponseDto(name, amount);

        //then
        //assertj 테스트 검증 라이브러리의 메서드
        //메서드 인자는 검증하고 싶은 대상
        assertThat(dto.getName()).isEqualTo(name);
        assertThat(dto.getAmount()).isEqualTo(amount);
        //getter와 생성자가 존재한다는게 증명됨
    }
}
```

assertj의 assertThat메서드를 사용한 이유

​	CoreMatchers와 달리 추가적인 라이브러리 x

​	자동완성 지원이 편리하다



### 객체지향 프로그래밍에서 관계형 데이터베이스 다루기

```java
//누가봐도 user, group는 부모 자식 관계
User user = findUser();
Group group = user.getGroup();

//DB가 간섭하면 user따로 group따로 조회해야됨..
User user = userDao.findUser();
Group group = groupDao.findGroup(user.getGroupId());
```



개발자는 객체지향적으로 프로그래밍을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성해서 실행

Spring Data JPA

JPA <- Hibernate <- Spring DataJPA

JPA 인터페이스를 사용하려면 구현체인 Hibernate가 필요한데 Spring에서는 이를 한단계더 추상화 시킨Spring Data JPA 모듈을 사용한다

- 구현체 교체의 용이성
- 저장소 교체의 용이성

Spring Data JPA(Redis, MongoDB)등 하위 프로젝트들은 CRUD 인터페이스가 같다

**JPA특징**

- 객체지향, 관계형DB 모두 잘 이해해야함
- CRUD 직접 작성할 필요 없음
- 관계 표현, 상태와 행위 관리 편리
- 네이티브 쿼리만큼의 퍼포먼스



**Spring 프로젝트에 Spring Data JPA적용하기**

**Post.java**

```java
@Getter //롬복
@NoArgsConstructor //롬복, 기본 생성자 자동 추가 public Posts(){}
@Entity //JPA 어노테이션 실제 DB와 매칭 될 클래스
//SalesManager.java -> sales_manager table
public class Posts extends BaseTimeEntity {

    @Id //해당 테이블의 PK 필드
    @GeneratedValue(strategy = GenerationType.IDENTITY) //PK 생성 규칙
    private Long id;

    @Column(length = 500, nullable = false) //선언하면 해당 클래스의 필드는 컬럼이 됨, 기본값 외 변경하고 싶은 옵션이 있는 경우
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;

    private String author;

    @Builder //롬복 해당 클래스의 빌더 패턴 클래스를 생성
    //생성자 상단에 선언시 생성자에 포함된 필드만 빌더에 포함
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }

    public void update(String title, String content) {
        this.title = title;
        this.content = content;
    }
}

```



Entity클래스에는 절대 Setter 메소드를 만들지 않는다.

값을 채울 필요가 있을때는 명확한 동작을 표현하는 메서드를 만들어서 사용

기본값은? 생성자를 사용하거나 Builder 패턴을 사용한다

- Builder패턴을 사용하면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지가능

```java
Example.builder().a(a).b(b).build();
```



**PostRepository.java**

```java
import java.util.List;

public interface PostsRepository extends JpaRepository<Posts, Long> {

    // index 페이지에서 글 등록
    // SpringDataJpa에서 제공하지 않으면 직접 쿼리를 작성해도 된다
    @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
    List<Posts> findAllDesc();


}
```

Post 클래스로 DB를 접근하게 해줄 JpaRepository

일반적으로 Dao라고 부르는 DB Layer 접근자, JPA에서는 Repository

Entity클래스와는 같은 위치에 두자

- Entity클래스는 기본 Repository없이는 제대로 역활을 할 수 없기 때문에, 도메인 패키지에서 관리



**PostRepositoryTest.java**

```java
import java.time.LocalDateTime;
import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;

@RunWith(SpringRunner.class)
@SpringBootTest //H2 데이터베이스 자동 실행
public class PostsRepositoryTest {

    @Autowired
    PostsRepository postsRepository;

    @After //JUnit에서 Test가 끝날때마다 수행되는 메서드
    public void cleanup() {
        postsRepository.deleteAll();
    }

    @Test
    public void 게시글저장_불러오기() {
        //given
        String title = "테스트 게시글";
        String content = "테스트 본문";

        postsRepository.save(Posts.builder() //id값이 있으면 update, 아니면, insert
                .title(title)
                .content(content)
                .author("jojoldu@gmail.com")
                .build());

        //when
        List<Posts> postsList = postsRepository.findAll(); //테이블의 모든 데이터 조회

        //then
        Posts posts = postsList.get(0);
        assertThat(posts.getTitle()).isEqualTo(title);
        assertThat(posts.getContent()).isEqualTo(content);
    }

    @Test
    public void BaseTimeEntity_등록() {
        //given
        LocalDateTime now = LocalDateTime.of(2019, 6, 4, 0, 0, 0);
        postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());
        //when
        List<Posts> postsList = postsRepository.findAll();

        //then
        Posts posts = postsList.get(0);

        System.out.println(">>>>>>>>> createDate=" + posts.getCreatedDate() + ", modifiedDate=" + posts.getModifiedDate());

        assertThat(posts.getCreatedDate()).isAfter(now);
        assertThat(posts.getModifiedDate()).isAfter(now);
    }
}

```



**application.properties**

```
spring.profiles.include=real-oauth
spring.jpa.show_sql=true //실행된 쿼리 형태 보기
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect //h2로그를 mysql버전으로 변경
spring.h2.console.enabled=true
spring.session.store-type=jdbc
```



Service에서 비즈니스 로직을 처리해야 하는가?

Service는 트랜잭션, 도메인 간 순서 보장의 역활

<img src="C:\JAVA\spring web.png" alt="spring web" style="zoom:67%;" />

- Web Layer
  - 컨트롤러, JSP/Reemaker 등의 뷰 템플릿 영역, 필터, 인터셉터, 컨트롤러 어드바이스 등 외부 요청과 응답에 대한 전반적인 영역
- Service Layer
  - @Service에 사용되는 영역, Controller와 Dao의 중간 영역에서 사용됨
  - @Transactional이 사용되어야 됨
- Repository Layer
  - DB에 접근하는 영역, DAO
- Dtos
  - Dto는 계층간의 데이터 교환을 위한 객체, Dtos는 이들의 영역
  - Repository에서 결과로 념겨준 객체, 뷰 템플릿 엔진에서 사용될 객체
- Domain Model
  - 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있게 단순화 시킨 것
  - 택시 앱에서는 승차, 하차, 탑승, 요금 등
  - Entity가 사용된 영역
  - 무조건 DB와 연관이 있을 필요는 없다



**비즈니스 로직은 Domain에서!!!**

기존에 서비스로 처리하던 방식을 트랜잭션 스크립트

**Domain Model**

```java
@Transactional
public Order cancelOrder(int orderId){
    Order order = ordersRepository.findById(orderId);
    Billing billing = billingRepository.findByOrderId(orderId);
    Delivery delivery = deliveryReposityro.findByOrderId(orderId);
    
    delivery.cancel();
    
    order.cancel();
    billing.cancel();
    
    return order;
}
```

order, billing, delivery가 각자 자신의 이벤트를 처리, 서비스 메소드는 도메인간의 순서만 보장



**PostApiController.java**

```java
import java.util.List;
//빈 주입 @Autowired, setter, 생성자
@RequiredArgsConstructor //롬복, 생성자 자동생성, 생성자를 통한 Bean 주입
@RestController
public class PostsApiController {

    private final PostsService postsService;

    @PostMapping("/api/v1/posts")
    public Long save(@RequestBody PostsSaveRequestDto requestDto) {
        return postsService.save(requestDto);
    }

    @PutMapping("/api/v1/posts/{id}")
    public Long update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto) {
        return postsService.update(id, requestDto);
    }

    @DeleteMapping("/api/v1/posts/{id}")
    public Long delete(@PathVariable Long id) {
        postsService.delete(id);
        return id;
    }

    @GetMapping("/api/v1/posts/{id}")
    public PostsResponseDto findById(@PathVariable Long id) {
        return postsService.findById(id);
    }

    @GetMapping("/api/v1/posts/list")
    public List<PostsListResponseDto> findAll() {
        return postsService.findAllDesc();
    }
}

```



**PostsService.java**

```java
import java.util.List;
import java.util.stream.Collectors;

@RequiredArgsConstructor
@Service
public class PostsService {
    private final PostsRepository postsRepository;

    @Transactional
    public Long save(PostsSaveRequestDto requestDto) {
        return postsRepository.save(requestDto.toEntity()).getId();
    }

    @Transactional
    public Long update(Long id, PostsUpdateRequestDto requestDto) {
        Posts posts = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 사용자가 없습니다. id=" + id));

        posts.update(requestDto.getTitle(), requestDto.getContent());

        return id;
    }

    @Transactional
    public void delete (Long id) {
        Posts posts = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 사용자가 없습니다. id=" + id));

        postsRepository.delete(posts);
    }

    @Transactional(readOnly = true)
    public PostsResponseDto findById(Long id) {
        Posts entity = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 사용자가 없습니다. id=" + id));

        return new PostsResponseDto(entity);
    }

    @Transactional(readOnly = true) //트랜젝션 범위는 유지, 조회 기능만 남겨두어 속도가 개선됨
    public List<PostsListResponseDto> findAllDesc() {
        return postsRepository.findAllDesc().stream()
                .map(PostsListResponseDto::new)
            //.map(posts -> new PostListResponseDto(posts))
                .collect(Collectors.toList());
    }
}

```



**PostsListResponseDto.java**

--

**PostsSaveRequestDto.java**

```java
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Getter
@NoArgsConstructor
public class PostsSaveRequestDto {
    private String title;
    private String content;
    private String author;

    @Builder
    public PostsSaveRequestDto(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }

    public Posts toEntity() {
        return Posts.builder()
                .title(title)
                .content(content)
                .author(author)
                .build();
    }

}
```

Entity클래스를 Request/Response클래스로 사용하면 안됨!!

Requset/Response용 Dto는 View를 위한 클래스라 변경이 자주 필요, View Layer와 DB Layer는 역활을 철저히 구분하는게 좋다

Entity클래스와 Controller에서 사용할 Dto는 구분해서 사용하자



**PostApiControllerTest.java**

```java
import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;
// For mockMvc

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {

    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private PostsRepository postsRepository;

    @Autowired
    private WebApplicationContext context;

    private MockMvc mvc;

    @Before
    public void setup() {
        mvc = MockMvcBuilders
                .webAppContextSetup(context)
                .apply(springSecurity())
                .build();
    }

    @After
    public void tearDown() throws Exception {
        postsRepository.deleteAll();
    }

    @Test
    @WithMockUser(roles="USER")
    public void Posts_등록된다() throws Exception {
        //given
        String title = "title";
        String content = "content";
        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
                .title(title)
                .content(content)
                .author("author")
                .build();

        String url = "http://localhost:" + port + "/api/v1/posts";

        //when
        mvc.perform(post(url)
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(title);
        assertThat(all.get(0).getContent()).isEqualTo(content);
    }

    @Test
    @WithMockUser(roles="USER")
    public void Posts_수정된다() throws Exception {
        //given
        Posts savedPosts = postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());

        Long updateId = savedPosts.getId();
        String expectedTitle = "title2";
        String expectedContent = "content2";

        PostsUpdateRequestDto requestDto = PostsUpdateRequestDto.builder()
                .title(expectedTitle)
                .content(expectedContent)
                .build();

        String url = "http://localhost:" + port + "/api/v1/posts/" + updateId;

        //when
        mvc.perform(put(url)
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
    }
}

```



### TestRestTemplate ?



### 테스트 코드 환경의 동작 원리?




