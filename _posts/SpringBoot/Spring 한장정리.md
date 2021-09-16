## INDEX

[TOC]



양식

문제

링크

요약





### Spring이란 무엇인가요?



*https://jerryjerryjerry.tistory.com/62*

Spring 이란

- JAVA 웹 프레임워크, Spring이란 JAVA 기술들을 더 쉽게 사용할 수 있게 해주는 오픈소스 프레임워크
- JAVA를 이용한 기술은 JSP, MyBatis, JPA등 많은데 이들을 더 편하게 사용하기 위해 만들어진 것



> 프레임 워크란 - 기본적인 설계나 필요한 라이브러리는 제공, 개발자는 만들고 깊은 기능을 구현하는데 집중 가능



*토비의 스프링*

- 스프링은 자바 엔터프라이즈 애플리케이션 개발에 사용되는 애플리케이션 프레임워크
- 애플리케이션 프레임워크는 애플리케이션 개발을 빠르고 효율적으로 할 수 있도록 애플리케이션의 바탕이 되는 틀과 공통 프로그래밍 모델, 기술 API등을 제공
- 틀- 스프링 컨테이너
  - 또는 애플리케이션 컨텍스트 라고 불리는 스프링 런타임 엔진을 제공
- 공통 프로그래밍 모델
  - 애플리케이션 코드가 생성되고 동작하는 방식에 대한 틀, 코드가 어떻게 작성돼야 하는지에 대한 기준 제시
  - IoC/DI
    - 유연하고 확장성 뛰어난 코드를 만들 수 있게 도와주는 객체지향 설계 원칙과 디자인 패턴의 핵심 원리를 담고 있다
  - 서비스 추상화
    - 구체적인 기술과 환경에 종속되지 않도록 유연한 추상 계층을 둔다
  - AOP
    - 애플리케이션 코드에 산재해서 나타다는 부가적인 기능을 독립적으로 모듈화 한다

- 기술 API
  - UI, 웹 프레젠테이션 계층, 비즈니스 서비스 계층, 기반 서비스 계층 도메인 계층, 데이터 액세스 계층 등에서 필요한 주요 기술을 스프링에서 이로간된 방식으로 사용할 수 있도록 지원해주는 기능과 전략 클래스 제공

스프링으로 개발한다는 것은 위의 3가지를 적극적으로 활용해서 애플리케이션을 개발한다는 뜻

스프링은 유연성과 확장성, 다양성에 무게, 집착에 가까울 정도로 모든 기능을 다양한 방법으로 확장하도록 설계되어 있다.



POJO 란

- JAVA EE를 사용하면서 해당 플랫폼에 종속되어 있는 무거운 객체들에 반발
- public HelloServlet extends HttpServlet  이런식으로 서블릿 프로그래밍 할때 반드시 HttpServlet을 상속받아야 하는 문제
- getter/setter를 가진 단순한 자바 오브젝트



Bean 기본 Singleton인 이유

- 3명의 유저가 접속했을때 3개의 스레드가 요청을 보낼텐데 이때 Controller와 Service, DAO 객체들이 각각 생성되는 것이 아니라 하나만 생성되고 스레드들은 이 객체를 공유하게 된다. 이 방법이 효율적



### Spring boot & Spring 차이점



### Spring Framework 기능 요소





### MVC란?

https://m.blog.naver.com/jhc9639/220967034588

https://coding-factory.tistory.com/69





### Spring MVC

토비의 스프링

- 스프링이 직접 제공하는 서블릿 기반의 MVC 프레임워크
- 특정 기술이나 방식에 매이지 않으면서 웹 프레젠테이션 계층의 각종 기술을 조합, 확장해서 사용할 수 있는 매우 유연한 웹 애플리케이션 개발의 기본 틀 제공
- 스프링이 제공하는 다양한 확장 포인트를 사용해서 기본적인 MVC 프레임워크를 만들어 둠
- 스프링 웹기술은 MVC 아키텍처를 근간으로 하고 보통 프론트 컨트롤러 패턴과 함께 사용된다 이 역할을 하는 DispatcherServlet을 핵심 엔진으로 사용
- 프론트 컨트롤러는 클라이언트가 보낸 요청을 받아서 공통적인 작업을 먼저 수행한 후 적절한 세부 컨트롤러로 작업을 위임, 예외 처리
- 

![a](C:\Users\heechang\Desktop\a.png)



**HTTP 클라이언트로 부터 요청을 받아서 응답하기까지 과정**

- DispatcherServlet의 HTTP 요청 접수
  - 자바 서블릿 컨테이너는 HTTP프로토콜을 통해 들어오는 요청이 DispacherServlet에 할당된 것이라면 HTTP 요청 정보를 DispacherServlet에 전달해 준다(web.xml에 정의됨)
  - 모든 요청에 대해 공통적으로 진행해야 하는 전처리 작업이 있다면 이를 수행, 보안, 파라미터 조작, 디코딩 등
- DispacherServlet에서 컨트롤러로 HTTP 요청 위임
  - 핸들러 매핑 전략을 사용해서 어떤 컨트롤러에게 작업을 위임할지 결정
  - 어떤 URL이 들어오면 어떤 컨트롤러 오브젝트가 이를 처리하게 할지 매핑해주는 전략을 만들어 DI로 제공
  - 그런다음 DispacherServlet이 컨트롤러의 특정 메소드를 실행해야하는데 이 컨트롤러는 특정 인터페이스로 구현 될 필요가 없다. 어떤 컨트롤러라도 사용 가능하다
  - 이를 구현하기 위해서 Adapter패턴을 사용 해당 컨트롤러 타입을 지원하는 어댑터를 중간에 껴서 호출
  - 이런 어댑터를 통한 컨트롤러 호출 방식을 통해 확장성 제공 가능
  - HandlerAdapter에 전달할때는 모든 웹 요청 정보가 있는 HttpServletRequest 타입의 오브젝트 전달 이를 어댑터가 적절히 변환해서 컨트롤러의 메소드가 받을 수 있는 파라미터로 변환해 줌 HttpServletResponse도 전달
- 컨트롤러의 모델 생성과 정보 등록
  - 컨트롤러의 작업은 사용자 요청을 해석하는 것, 그에 따라 실제 비즈니스 로직을 수행하도록 서비스 계층 오브젝트에게 작업을 위임하는 것, 그 결과로 모델을 생성하는 것, 마지막으로 어떤 뷰를 사용할지 결정하는 것
  - 컨트롤러가 무조건 돌려줘야하는 정보 두가지, 모델, 뷰
  - 모델은 맵에 담긴 정보, 이름에 대응되는 값의 쌍
- 컨트롤러의 결과 리턴, 모델, 뷰
  - 컨트롤러가 뷰 오브젝트를 직접 리턴할 수도 있지만 보통은 뷰의 논리적인 이름을 리턴해주면 DispatcherServlet의 전략인 뷰 리졸버가 이를 이용해 뷰 오브젝트를 생성 해 준다
  - 예를 들어 JSP 를 사용하는 경우 JSP 템플릿 파일의 이름을 리턴해 주어야 한다
  - 스프링에는 ModelAndView라는 오브젝트가 있는데 DispatcherServlet이 최종적으로 어댑터를 통해 컨트롤러로부터 돌려받는 오브젝트이다.
- DispatcherServlet의 뷰 호출과 모델 참조
  - 컨트롤러부터 모델 과 뷰를 받으면 뷰 오브젝트에게 모델을 전달해주고 클라이언트에게 돌려줄 최종 결과물을 생성해달라고 요청하는 것
  - 뷰 작업을 통한 최종 결과물을 HttpServletResponse 오브젝트 안에 담긴다
- HTTP 응답 돌려주기
  - 뷰 생성까지 마쳤으면 DispatcherServlet 은 등록된 후처리를 하고 HttpServletResponse에 담긴 최종 결과를 서블릿 컨테이너에게 돌려준다 서블릿 컨테이너는 HttpServletResponse에 담긴 정보를 HTTP 응답으로 만들어서 클라이언트에 전송



**DispatcherServlet의 DI가능한 전략, 확장 가능한 전략**

HandlerMapping

- URL과 요청 정보를 기준으로 어떤 핸들러 오브젝트, 즉 컨트롤러를 사용할 것인지 결정하는 로직 담당
- HandlerMapping interface를 구현해서 만들 수 있고 DispatcherServlet는 여러개의 핸들러 매핑을 가질 수 있다

HandlerAdapter

- 핸들러 매핑으로 선택한 컨트롤러/핸들러를 DispatcherServlet이 호출할 때 사용하는 어댑터
- 컨트롤러 타입에는 제한이 없고 컨트롤러 호출방법은 DispatcherServlet가 알 필요가 없다 컨트롤러 타입에 적함한 어댑터를 가져다가 이를 통해 컨트롤러 호출
- Default는 HttpRequestHandlerAdapter..

HandlerExceptionResolver

- 예외가 발생했을 때 이를 처리하는 로직
- 에러 페이지 표시나, 관리자에게 통보해주는 작업은 프론트 컨트롤러인 DispatcherServlet 에서 처리되어야 하는데 HandlerExceptionResolver 중에서 발생한 예외에 적합한 것을 찾아서 예외처리를 위임

ViewResolver

- 컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 전략 오브젝트
- 뷰 종류에 따른 다양한 뷰 리졸버 추가가능

LocaleResolver

- 지역 정보를 결정해주는 전략

ThemeResolver

- 테마를 가지고 이를 변경해서 사이트를 구성, 테마 정보를 결정해 주는 전략

RequestToViewNameTranslator

- 컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략



### 컨트롤러의 역활

- 사용자의 요청 분석
- 사용자의 요청 검증, 예외처리
- 요청을 적절한 형태로 가공, 변환
- 서비스로부터 결과를 받으면 어떤 뷰를 보여줘야 하는지 결정
- 뷰에 출력할 내용을 모델에 적절한 이름으로 넣어줘야 한다



**스프링 MVC가 제오하는 컨트롤러의 종류와 그에 따른 핸들러 어댑터**

Servlet과 SimpleServletHandlerAdapter

- 표준 서블릿 인터페이스인 javax.servlet.Servlet을 구현한 서블릿 클래스
- 기존에 서블릿으로 개발된 코드를 스프링 MVC에 가져올때 사용

HttpRequestHandler와 HttpRequestHandlerAdapter

- 전형적인 서블릿 스펙을 준수할 필요 없이 HTTP 프로토콜을 기반으로 한 전용 서비스를 만들려고 할때 사용

Controller와 SimpleControllerHandlerAdapter

- DispatcherServlet이 컨트롤러와 주고받는 정보를 그대로 메소드의 파라미터와 리턴 값으로 가지고 있다, 가장 기본적인 타입
- 스프링 MVC를 확장해서 애플리케이션에 최적화된 전용 컨트롤러를 설계할 때 가장 유용하다

AnnotationMethodHandlerAdapter

- 컨트롤러의 타입이 정해져 있지 않다
- 클래스와 메소드에 붙은 몇 가지 애노테이션의 정보와 메소드 이름, 파라미터, 리턴 타입에 대한 규칙 등을 종합적으로 분석해서 컨트롤러를 선별하고 호출 방식을 결정한다. 굉장히 유연!!
- 컨트롤러 하나가 하나 이상의 URL에 매핑 가능, URL 매핑을 컨트롤러 단위가 아니라 메소드 단위로 가능하게 했다
- DefaultAnnotationHandlerMapping 핸들러 매핑과 함꼐 사용해야 한다
- 규칙과 관례를 잘 기억해야 한다



**핸들러 매핑**

- 컨트롤러를 찾아주는 기능을 가진 DispatcherServlet의 전략
- 컨트롤러의 타입과는 상관 없다



BeanNameUrlHandlerMapping

- 빈의 이름에 들어 있는 URL을 HTTP 요청의 URL과 비교해서 일치하는 빈을 찾아준다. 와일드 카드 사용 가능

ControllerBeanNameHandlerMapping

- 빈의 아이디나 빈 이름을 이용해 매핑해주는 핸들러 매핑 전략
- 이름 앞 뒤의 prefix, suffix 지정 가능



ControllerClassNameHandlermapping

- 빈 이름 대신 클래스 이름을 URL에 매핑해 주는 핸들러 매핑 클래스



SimpleUrlHandlerMapping

- URL과 컨트롤러의 매핑정보를 한곳에 모아놓을 수 있는 핸들러 매핑 전략
- 매핑정보는 빈 등록정보의 프로퍼티에 넣어준다



DefaultAnnotationHandlerMapping

- @RequestMapping 이라는 애노테이션을 컨트롤러 클래스나 메소드에 직접 부여하고 이를 이용해 매핑
- 메소드 단위로 매핑 가능
- HTTP 메소드, HTTP 헤더정보까지 매핑에 활용 가능
- 정책과 작성 기준을 잘 만들어 두어야 함



핸들러 매핑의 다양한 프로퍼티

order

- 하나의 애플리케이션 안에서 두 개 이상의 핸들러 매핑을 함계 사용하는 경우 우선순위 지정

defaultHandler

- URL을 매핑할 대상을 찾지 못했을 경우 자동으로 디폴트 핸들러를 선택해 준다

- HTTP404 대신 안내 메시지 뿌려주기 위해 사용

alwaysUseFullPath

- 기본적으로 URL 매핑은 컨텍스트 패스와 서블릿 패스 두가지를 제외하고 나머지만 비교
- 웹 애플리케이션의 배치 경로와 서블릿 매핑을 변경하더라도 URL 매핑정보가 영향받지 않도록 하기 위함
- 특별한 경우 URL의 전체 경로를 매핑하기 위해서 사용

detectHandlerslnAncestorContext



### DispatcherServlet의 동작과정

**Request 처리하는 과정**



**url이 맵핑되는 정보가 초기화 되는 시점**

- 전략 빈들이 주입되는 시점
- RequestMappingHandlerMapping에서 ApplicationContext 내부의 모든 빈들을 가져와서 핸들러로 유효한 빈에 대해  DetectHandlerMethods를 실행시켜 mappingRegistry에 추가한다





### @Controller와 @RestController의 차이점

https://mangkyu.tistory.com/49



@Controller 사용

- 주로 View를 반환하기 위해 사용
- @ResponseBody 를 사용해서 Data 반환도 가능 이 경우에 viewResolver 대신에 HttpMessageConverter가 동작



@RestController

- Spring MVC Controller에 @ResponseBody가 추가된것
- Json 형태로 Data 반환





### JPA, Spring Data JPA, Hibernate

https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/

ORM

DB의 데이터를 객체로 매핑시켜 데이터를 접근, JPA, Hibernate등이 해당



Mybatis(SQL Mapper)

- Java는 DB에 접근하는 JDBC 라이브러리를 제공

- Connection을 생성해주고 불필요한 부분을 쉽게 해주는것이 Mybatis



JPA는 Java ORM 기술에 대한 API 표준 명세 즉 인터페이스

Hibernate는 JPA의 구현체

Spring Data JPA는 JPA 구현체들을 쓰기 좋게 만들어 놓은 모듈, JPA 를 한단계 더 추상화 시킨 Repository라는 인터페이스를 제공

- 다른 구현체로 변경하기가 용이
- Hibernate를 직접 사용하는것 보다 Spring Data 구현체를 사용하는게 권장됨



### Filter, Interceptor, AOP의 차이

https://goddaehee.tistory.com/154

https://www.leafcats.com/39#:~:text=Interceptor(%EC%9D%B8%ED%84%B0%EC%85%89%ED%84%B0)%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%9C%EB%8B%A4.&text=1.%20Filter%EB%8A%94%20Dispatcher%20servlet,%EC%A0%84%EC%97%90%20%EC%A0%95%EB%B3%B4%EB%A5%BC%20%EC%B2%98%EB%A6%AC%ED%95%9C%EB%8B%A4.



### OAuth2 인증

https://velog.io/@piecemaker/OAuth2-%EC%9D%B8%EC%A6%9D-%EB%B0%A9%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90#:~:text=%EB%8C%80%ED%95%B4%20%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90.-,OAuth%202.0%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D,%ED%95%98%EB%8A%94%20%EA%B2%83%EC%9D%B4%20%EC%95%84%EB%8B%88%EB%9D%BC%EB%8A%94%20%EB%9C%BB%EC%9D%B4%EB%8B%A4.



