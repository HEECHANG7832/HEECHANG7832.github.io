# 채용 준비

JPA Java Persistent API

RESTful 

- MSA(Spring Cloud) 구조 설계 및 개발

- Spring Cloud, Open APIs, JPA 기반 개발 및 운영
- DevOps 운영

- JPA, Hibernate 등 ORM 에 대한 이해가 있는 분

- MySQL 등 RDBMS 에 대한 이해가 있는 분

- e-커머스 플랫폼에 대한 관심이 많은 분
- 대용량 데이터 처리에 대해 관심이 있는 분



- 문제는 프로그래밍 문제 3문항으로 구성됩니다.
- 프로그래밍 문제 풀이에 사용 가능한 언어는 Java입니다.



코딩하고 블로깅하고

레퍼런스는 가볍게 봐야함

가성비있게 공부하자







# Web Framework

**웹 프레임워크**(Web framework) 또는 **웹 애플리케이션 프레임워크**(Web application framework)는 웹 서비스 개발을 위한 [프레임워크](https://namu.wiki/w/프레임워크)이다. [Java](https://namu.wiki/w/Java)의 [Spring](https://namu.wiki/w/Spring(프레임워크)), [Python](https://namu.wiki/w/Python)의 [Django](https://namu.wiki/w/Django), [Node.js](https://namu.wiki/w/Node.js)의 Express, [PHP](https://namu.wiki/w/PHP)의 [Laravel](https://namu.wiki/w/라라벨), [Ruby](https://namu.wiki/w/Ruby)의 [Ruby on Rails](https://namu.wiki/w/Ruby on Rails) 등이 특히 유명하다. 웹 프레임워크를 사용하면 쉽고 빠르게 [웹사이트](https://namu.wiki/w/웹사이트)를 만들 수 있다. ~~Spring은 예외인 듯하다~~

Spring이나 Django, Ruby on Rails의 경우 풀 스택(Full-stack) 웹 프레임워크이다. 풀 스택은 "모든 분야에 다 능숙한"이라는 의미로, 풀 스택 웹 프레임워크면 웹 개발에 필요한 요소를 모두 갖춘 웹 프레임워크이다. 풀 스택 웹 개발자는 [프론트엔드](https://namu.wiki/w/프론트엔드)와 [백엔드](https://namu.wiki/w/백엔드) 개발이 모두 가능한 개발자를 말한다.



# Spring



[Java](https://namu.wiki/w/Java)/[Kotlin](https://namu.wiki/w/Kotlin) 기반의 [웹 프레임워크](https://namu.wiki/w/웹 프레임워크).[[1\]](https://namu.wiki/w/Spring(프레임워크)#fn-1) 

스프링이름의 유래는 이전에 Java EE(엔터프라이즈 에디션)의 스펙을 구현한 EJB가 기술의 복잡도가 증가해서 성능이 느렸던것을 탈피해서 EJB시절을 “겨울”에 빗대어 겨울 후의 “봄”으로 새로운 시작을 의미로 스프링(봄)이 되었다. [Java Virtual Machine](https://namu.wiki/w/Java Virtual Machine)에서 작동하며, [아파치 라이선스](https://namu.wiki/w/아파치 라이선스) 2.0을 따르는 [오픈 소스](https://namu.wiki/w/오픈 소스) [프레임워크](https://namu.wiki/w/프레임워크)이다.



- **POJO(Plain Old Java Object) 방식** : POJO는 Java EE의 EJB 를 사용하면서 해당 플랫폼에 종속되어 있는 무거운 객체들을 만드는 것에 반발하며 나타난 용어다. 별도의 프레임워크 없이 Java EE를 사용할 때에 비해 특정 인터페이스를 직접 구현하거나 상속받을 필요가 없어 기존 라이브러리를 지원하기가 용이하고, 객체가 가볍다.

- POJO는 gettet/setter를 가진 단순 자바 오브젝트로 정의를 하고 있습니다. 이러한 단순 오브젝트는 의존성이 없고 추후 테스트 및 유지보수가 편리한 유연성의 장점을 가집니다.

- **관점 지향 프로그래밍(Aspect Oriented Programming, AOP)** : 로깅, 트랜잭션, 보안 등 여러 모듈에서 공통적으로 사용하는 기능을 분리하여 관리할 수 있다. AspectJ를 포함하여 사용할 수 있고, 스프링에서 지원하는 실행에 조합하는 방식도 지원한다. 이 분리관리한다는게 개념이 처음에 이해해기가 어려운데, 추상/부모/클래스나 인터페이스로 관리된다는게 아니라 모듈을 관리해주는 모듈을 상하/인터페이스 관계없이 따로 마련한다는 개념에 가깝다. 더 쉽게 이야기하자면 군대에서 보급품을 받는다고 가정하자. 상급부대(연대, 사단)에서 보급품을 내려받는게 아니라. 국군복지단이나/군수사령부 아저씨가 직접 가져오는것을 생각해보면 쉽다. 당연히 군수사령부 여하부대 아저씨도 대대소속이므로 상하관계가 없지만 보급품에 한해서만 배부해주는 것이다. 전공자들을 위해서 더 쉽게 설명하자면 [C언어](https://namu.wiki/w/C언어)에서는 중복할당을 줄이기 위해서 간접적으로 값을 가르키는 **포인터**를 가르키는데, Spring에서는 반복할당을 줄이기 위해 **포인터를 대신하여** 스프링 어노테이션을 사용하는 것이라고 보면 된다.

- DL(Dependency Lookup) - 의존성 검색 // 컨테이너에서는 객체들을 관리하기 위해 별도의 저장소에 빈을 저장하는데 저장소에 저장되어 있는 개발자들이 컨테이너에서 제공하는 API 를 이용하여 사용하고자 하는 빈 을 검색하는 방법입니다.

- **의존성 주입(Dependency Injection, DI)** : 프로그래밍에서 구성요소 간의 의존 관계가 소스코드 내부가 아닌 외부에서 설정을 통해 정의되는 방식이다. 코드 재사용을 높여 소스코드를 다양한 곳에 사용할 수 있으며 모듈간의 결합도도 낮출 수 있다. 계층, 서비스 간에 의존성이 존재하는 경우 스프링 프레임워크가 서로 연결시켜준다.

- ```java
  //DI 예시
  //두개의 어노테이션으로 MyService 객체의 인스턴스를 쉽게 얻을 수 있습니다. 이는 결합도를 낮춘 것입니다. 스프링 프레임워크는 이렇게 일을 단순하게 만들기 위한 방법을 제공합니다.
  @Component
  
  public class MyService {
  
      public String retrieveWelcomeMessage(){
  
         return "Welcome to InnovationM";
  
      }
  
  }
  
  @RestController
  
  public class MyController {
  
      @Autowired
  
      private MyService service;
  
  
  
      @RequestMapping("/welcome")
  
      public String welcome() {
  
          return service.retrieveWelcomeMessage();
  
      }
  
  }
  ```

- 

- **제어 역전(Inversion of Control, IoC)** : 전통적인 프로그래밍에서는 개발자가 작성한 프로그램이 외부 라이브러리의 코드를 호출해서 이용했다. 제어 역전은 이와 반대로 외부 라이브러리 코드가 개발자의 코드를 호출하게 된다. 즉, 제어권이 프레임워크에게 있어 필요에 따라 스프링 프레임워크가 사용자의 코드를 호출한다.

- **생명주기 관리** : 스프링 프레임워크는 Java 객체의 생성, 소멸을 직접 관리하며 필요한 객체만 사용할 수 있다.

- **Core** : 제어 역전(IoC)과 의존성 주입(DI) 기능을 제공한다. 생소한 용어일 수 있으나 제어 역전은 전체적인 프로세스의 흐름이 개발자가 아니라 프레임워크(여기서는 Spring)에 의해 결정된다는 뜻이다. 개발자는 프레임워크가 정한 틀에 따라 적절한 코드를 작성해 넣기만 하면 되기 때문이다. 의존성 주입은 객체 생성에 관한 뜻이다. 클래스 A와 B가 있다고 할 때, A 클래스의 메소드 내에서 B 클래스의 객체를 생성하여 비즈니스 로직에 사용하면 A는 B에 '의존'하는 관계가 된다. 그리고 A, B 클래스가 아닌 외부에서 A 클래스의 메소드를 호출하고, 파라미터 값으로 B 클래스의 객체를 전달한다면 이것은 '주입'이 된다. 그렇다면 의존성 주입은? 이 두 상황을 합치면 된다. 파라미터 값으로 전달받은 B 객체를 A 클래스의 메소드 내에서 비즈니스 로직에 사용하는 것을 의미한다. 즉 A와 B의 '의존' 관계가 **외부**에서의 '주입'을 통해 이루어진 것이다.
- **DAO** : JDBC 추상 계층을 제공한다. JDBC는 자바의 데이터베이스 커넥터이다.[[3\]](https://namu.wiki/w/Spring(프레임워크)#fn-3) 데이터가 담겨있는 VO(Value Object) 클래스를 이용해 사용한다.
- **ORM** : JPA, Hibernate와 같은 ORM이나 MyBatis 같은 데이터베이스 API 등과 통합할 수 있는 기능을 제공한다.
- **AOP** : 스프링 프레임워크에서 제공하는 AOP 패키지를 제공한다. 공통로직을 한군데서 관리해서 공동으로 사용한다는 개념 자체는 어렵지 않으나. 데이터와 변수가 어디서 어디로 오고가는지를 따지면 머리통이 돌아버리게 된다. 스프링 공부하는 도중 최악의 난이도를 지닌 구간이라고 할 수 있다. 처음 공부할 때는 대충 보고 뒤의 내용을 계속 공부하는 것을 추천한다. 실질적으로는 로그찍기용이 대부분이다.
- **Web** : Spring Web MVC, Struts, WebWork 등 웹 어플리케이션 구현에 도움되는 기능을 제공한다.
- **JEE** : EJB, JMX 등의 엔터프라이즈 J2EE 스펙에 관한 기능을 제공한다.



**Spirng Framework는 경량 컨테이너로 자바 객체를 담고 직접 관리**합니다. 객체의 생성 및 소멸 그리고 라이프 사이클을관리하며 언제든 Spring 컨테이너로 부터 필요한 객체를 가져와 사용할 수 있습니다. 

**제어의 흐름을 사용자가 컨트롤 하지 않고 위임한 특별한 객체에 모든 것을 맡기는 것입니다.**

**IOC란 기존 사용자가 모든 작업을 제어하던 것을 특별한 객체에 모든 것을 위임하여 객체의 생성부터 생명주기 등 모든 객체에 대한 제어권이 넘어 간 것을 IOC, 제어의 역전 이라고 합니다.**







# Spring boot



스프링이란 무엇인가?

스프링은 의존성 주입 같은 특징들을 가지고 있어서 개발 기간을 많이 단축시켜 준다

- Spring JDBC
- Spring MVC
- Spring Security
- Spring AOP
- Spring ORM
- Spring Test

이전 web 개발시에 boilerplate code들을 많이 삽입했





https://www.baeldung.com/spring-vs-spring-boot

그럼 스프링 부트란?



Transaction Manager, Hibernate Datasource, Entity Manager, Session Factory와 같은 설정을 하는데에 어려움이 많이 있었습니다. 최소한의 기능으로 Spring MVC를 사용하여 기본 프로젝트를 셋팅하는데 개발자에게 너무 많은 시간이 걸렸습니다.**스프링부트는 스프링의 이러한 설정에 관한 이슈를 어떻게 풀었을까??**

1.

스프링부트는 자동설정(AutoConfiguration)을 이용하였고 어플리케이션 개발에 필요한 모든 내부 디펜던시를 관리합니다. 개발자가 해야하는건 단지 어플리케이션을 실행할 뿐입니다. 스프링의 jar파일이 클래스 패스에 있는 경우 Spring Boot는 Dispatcher Servlet으로 자동 구성합니다. 마찬가지로 만약 Hibernate의 jar파일이 클래스 패스내에 존재한다면 이를 datasource로 자동설정하게 됩니다. 스프링부트는 미리설정된 스타터 프로젝트를 제공합니다.

 

2.

웹어플리케이션을 개발하는 동안, 우리는 사용하려는 jar, 사용할 jar 버전, 함께 연결하는 방법이 필요합니다. 모든 웹 어플리케이션에는 Spring MVC, Jackson Databind, Hibernate 코어 및 Log4j와 같은 유사한 요구사항들이 있습니다. 그래서 우리는 이러한 jar들의 서로 호환되는 버전들을 따로 선택을 해주어야 했습니다. 이런 복잡도를 줄이기 위해서 스프링 부트는 SpringBoot Starter라고 불리는 것을 도입했습니다

당신이 한번 스타터 Dependency를 추가하면 스프링부트 스타터 웹프로젝트가 pre-packaged된 형태로 제공됩니다. 그로인해 개발자는 Dependency 관리와 호환버전에 대하여 고려할 필요가 없습니다.



#### **스프링 부트 스타터 프로젝트 옵션들**

응용프로그램의 종류에 따라 쉽고 빠르게 개발을 도와줄 수 있는 몇가지 스프링 스타터 프로젝트를 소개하겠습니다

\* spring-boot-starter-web-services : SOAP 웹 서비스

\* spring-boot-starter-web : Web, RESTful 응용프로그램

\* spring-boot-starter-test : Unit testing, Integration Testing

\* spring-boot-starter-jdbc : 기본적인 JDBC

\* spring-boot-starter-hateoas : HATEOAS 기능을 서비스에 추가

\* spring-boot-starter-security : 스프링 시큐리티를 이용한 인증과 권한

\* spring-boot-starter-data-jpa : Spring Data JPA with Hibernate

\* spring-boot-starter-cache : 스프링 프레임워크에 캐싱 지원 가능

\* spring-boot-starter-data-rest : Spring Data REST를 사용하여 간단한 REST 서비스 노출





스프링 프레임워크의 확장

스프링 app을 셋업하기 위해 요구되는 boilerplate configuration을 제거



스프링부트의 특징

- Opinionated ‘starter' dependencies to simplify the build and application configuration
- Embedded server to avoid complexity in application deployment
- Metrics, Health check, and externalized configuration
- Automatic config for Spring functionality – whenever possible



Maven dependencies 의 dependencies 변화

spring-web, spring-webmvc

--> spring-boot-starter-web

Test 코드 작성시에도 JUnit같은 모든 library를 더하는 대신에 스프링 부트에서는 starter dependency가 자동으로 포함시켜준다

여러가지 starter 모듈들

- *spring-boot-starter-data-jpa*
- *spring-boot-starter-security*
- *spring-boot-starter-test*
- *spring-boot-starter-web*
- *spring-boot-starter-thymeleaf*



https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-documentation

https://henry23.tistory.com/148

-------------------------------------------------------------------



### MVC

**Model**

Model에서는 데이터처리를 담당하는 부분입니다. Model부분은 Serivce영역과 DAO영역으로 나누어지게 되고 여기서 중요한 것은 Service 부분은 불필요하게 HTTP통신을 하지 않아야하고 request나 response와 같은 객체를 매개변수로 받아선 안된다. 또한 Model 부분의 Service는 view에 종속적인 코드가 없어야 하고 View 부분이 변경되더라도 Service 부분은 그대로 재사용 할 수 있어야 한다.

**Model에서는 View와 Controller 어떠한 정보도 가지고 있어서는 안된다.**

**View**

View는 사용자 Interface를 담당하며 사용자에게 보여지는 부분입니다. View는 Controller를 통해 모델에 데이터에 대한 시각화를 담당하며 View는 자신이 요청을 보낼 Controller의 정보만 알고 있어야 하는 것이 핵심이다.

**Model이 가지고 있는 정보를 저장해서는 안되며 Model, Controller에 구성 요소를 알아서는 안된다.**



**Controller**

Controller에서는 View에 받은 요청을 가공하여 Model(Service 영역)에 이를 전달한다. 또한 Model로 부터 받은 결과를 View로 넘겨주는 역할을 합니다. Controller에서는 모든 요청 에러와 모델 에러를 처리하며 View와 Controller에 정보를 알고 있어야한다.

**Model과 View의 정보에 대해 알고 있어야한다.**



Model의 Service영역은 자신을 어떠한 Controller가 호출하든 상관없이 정해진 매개변수만 받는다면 자신의 비즈니스 로직을 처리할 수 있어야한다. 

즉, 모듈화를 통해 어디서든 재사용이 가능하여야 한다는 뜻이다. 

이말은 View의 정보가 달라지더라도 Controller에서 Service에 넘겨줄 매개변수 데이터 가공만 처리하면 되기 때문에 유지보수 비용을 절감 할 수 있는 효과가 있다. 또한 Service영역의 재사용이 용이하기 때문에 확장성 부분에서도 큰 효과를 볼 수 있는 장점이있다.

**MVC의 처리 순서**

1. 클라이언트가 서버에 요청을 하면, front controller인 DispatcherServlet 클래스가 요청을 받는다.
2. DispatcherServlet는 프로젝트 파일 내의 servlet-context.xml 파일의 @Controller 인자를 통해 등록한 요청 위임 컨트롤러를 찾아 매핑(mapping)된 컨트롤러가 존재하면 @RequestMapping을 통해 요청을 처리할 메소드로 이동한다.
3. 컨트롤러는 해당 요청을 처리할 Service(서비스)를 받아 비즈니스로직을 서비스에게 위임한다.
4. Service(서비스)는 요청에 필요한 작업을 수행하고, 요청에 대해 DB에 접근해야한다면 DAO에 요청하여 처리를 위임한다.
5. DAO는 DB정보를 DTO를 통해 받아 서비스에게 전달한다.
6. 서비스는 전달받은 데이터를 컨트롤러에게 전달한다.
7. 컨트롤러는 Model(모델) 객체에게 요청에 맞는 View(뷰) 정보를 담아 DispatcherServlet에게 전송한다.
8. DispatcherServlet는 ViewResolver에게 전달받은 View정보를 전달한다.
9. ViewResolver는 응답할 View에 대한 JSP를 찾아 DispatcherServlet에게 전달한다.
10. DispatcherServlet는 응답할 뷰의 Render를 지시하고 뷰는 로직을 처리한다.
11. DispatcherServlet는 클라이언트에게 Rendering된 뷰를 응답하며 요청을 마친다.



**DAO**

Data Access Object의 약자로, 데이터베이스의 데이터에 접근하기 위해 생성하는 객체이다.

데이터베이스에 접근하기 위한 로직과 비즈니스 로직을 분리하기 위해 사용한다.

 

간단하게, DB에 접속하여 데이터의 CRUD(생성, 읽기, 갱신, 삭제) 작업을 시행하는 클래스이다.

JSP 및 Servlet 페이지 내에 로직을 기술하여 사용할 수 있지만, 코드의 간결화 및 모듈화, 유지보수 등의 목적을 위해 별도의 DAO 클래스를 생성하여 사용하는 것이 좋다.

 

한 줄 요약 : DAO는 DB를 사용하여 데이터의 조회 및 조작하는 기능을 전담하는 오브젝트이다.

 

 

**DTO**

Data Transfer Object의 약자로, 계층간 데이터 교환을 위한 자바빈즈를 뜻한다.

또한 DTO는 VO(Value Object)와 용어를 혼용해서 많이 사용하는데, VO는 읽기만 가능한 read only 속성을 가져 DTO와의 차이점이 존재한다.

 

일반적으로 DTO는 로직을 가지고 있지 않은 순수한 데이터의 객체이며 객체의 속성과 그 속성의 접근을 위한 getter 및 setter 메소드만을 가지고 있다.

```
user_dao userdao = new user_dao();
ArrayList<user_dto> dtos = userdao.User_Select();
```



### RESTful 서비스









# Github연동

- .idea 디렉토리는 인텔리제이에서 자동으로 생성되는 파일
- .ignore  (다양한 이그노어 파일 지원하는 플러그인)



# 테스트 코드

- 테스트 코드를 사용한 기능테스트는 습관
- JAVA 테스트 코드 작성을 도와주는 JUnit

```java
//항상 프로젝트 최상단에 위치해야함

@SpringBootApplication
//스프링 부트의 자동 설정, Bean 읽기와 생성3
@RestController
public class HelloController {
     @GetMapping("/hello") //HTTP Method인 Get 요청을 받을수 있는 API
    public String hello() {
        return "hello";
    }
}

```



**스프링 실행자**





객체를 생성안하고 사용한다고?



HTTP GET, POST



HTTP 상태들 200(OK), 404, 500



assertThat(dto.getName()).isEqualTo(name);

assertj 테스트 검증 라이브러리의 검증 메소드



## Annotation

**@Component** : 스프링의 BeanFactory라는 팩토리 패턴의 구현체에서 bean이라는 스프링프레임워크가 관리하는 객체가 있는데 해당 클래스를 그러한 bean 객체로 두어 스프링 관리하에 두겠다는 어노테이션.



**@Autowired** : 스프링 프레임워크에서 관리하는 Bean 객체와 같은 타입의 객체를 찾아서 자동으로 주입해주는 것. 해당 객체를 Bean으로 등록하지 않으면 주입해줄 수 없다.



@Service

@Repository





---




