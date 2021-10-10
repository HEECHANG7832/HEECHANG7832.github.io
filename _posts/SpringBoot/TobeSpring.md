



[TOC]

## 토비의 스프링 Vol 1



분리와 확장을 고려한 코드 작성

### 관심사의 분리

-   관심이 같은 것끼리는 하나의 객체 안으로 모이게
-   관심이 다른 것은 가능한 따로 떨어져서 영향을 주지 않게

```
public void add(User user) throws ClassNotFoundException, SQLException{
    Class.forName("com.mysql.jdbc.Driver");
    Connection c = DriverManager.getConnection(
        "jdbc:mysql://localhost/springbook", "root", "rnjs1078"
    );
    PreparedStatement ps = c.prepareStatement(
        "insert into users(id, name, password) values(?,?,?)"
    );

    ps.executeUpdate();

    ps.close();
    c.close();
}
```

-   커넥션을 가져오는 부분
-   add와 get에 중복

### 템플릿 메소드 패턴

상속을 통해 슈퍼 클래스의 기능을 확장

-   변하지 않는 기능은 슈퍼 클래스에 만들어 두고
-   자주 변경되는 확장성은 서브클래스에서 구현한다

### 팩토리 메소드 패턴

슈퍼 클래스가 서브클래스가 구현할 메서드를 호출해서 필요한 타입의 오브젝트를 가져와 사용한다

-   서브클래스가 어떤 클래스 오브젝트를 만들어 리턴할지 관심없음
-   서브클래스가 다양한 방법으로 오브젝트 생성하고 클래스를 결정할 수 있도록 미리 정의해둔 메소드를 팩토리 메소드

```java
public abstract class UserDao {
    public void add(User user) throws ClassNotFoundException, SQLException{
        Connection c = getConnection();
        ///
    }

    public User get(String id) throws ClassNotFoundException, SQLException{
        Connection c = getConnection();
        ///
    }

    public abstract Connection getConnection() throws ClassNotFoundException, SQLException;
}

public class NUserDao extends UserDao{
    @Override
    public Connection getConnection() throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection c = DriverManager.getConnection(
                ///
        );
    }
}
```



객체지향의 다형성을 잘 활용하는법

- 클래스 사이의 관계는 클래스내부에 다른 클래스의 이름이 나타나기 때문
- 코드에서 특정 클래스를 전혀 알지 못해도 해당 클래스가 구현한 인터페이스를 사용했다면 오브젝트를 인터페이스 타입으로 받아서 사용할 수 있다.
- 모델링시에 없던 관계라도 런타임시 오브젝트끼리 관계를 형성시켜줄 수 있다.



연결과 관련된 부분은 ConnectionMaker로 분리

**ConnectionMaker.java**

```
public interface ConnectionMaker {
    public Connection makeConnection() throws ClassNotFoundException, SQLException;
}

public class DConnectionMaker implements ConnectionMaker{

    @Override
    public Connection makeConnection() throws ClassNotFoundException, SQLException {
        return null;
    }
}
```

UserDao에서는 Connection을 받아서 사용

```
public class UserDao {
    private ConnectionMaker connectionMaker;

    public UserDao(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }
    ///

}
```

**UserDaoTest.java**

```
public class UserDaoTest {
    public static void main(String[] args) throws ClassNotFoundException, SQLException{
        ConnectionMaker connectionMaker = new DConnectionMaker();

        UserDao dao = new UserDao(connectionMaker);

    }
}
```



N사든 D사든 Connection 방법이 변경되더라도 UserDao의 코드는 수정할 필요가 없다

해당하는 connection을 만들어서 넣어주면 되기 때문!!!

객체지향 설계 원칙 SOLID

- 단일 책임 원칙
- 개방 폐쇄 원칙
- 리스코프 치환 원칙
- 인터페이스 분리 원칙
- 의존관계 역전 원칙

###  

### 개방폐쇄 원칙

- 클래스나 모듈은 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다
  - DB연결방법이라는 기능을 확장하는데는 UserDao를 변경하지 않아도 확장가능
  - UserDao자신은 DB연결방법이 변경되는데에 핵심코드는 영향을 받지 않아 변경에는 닫혀있다
- 높은 응집도(하나의 모듈이 하나의 관심사)
  - 변경이 발생할때 많은 부분이 변경됨
- 낮은 결합도
  - 책임과 관심사가 다른 오브젝트 끼리는 느슨하게 연결된 상태
  - 하나의 변경이 일어날때 파도가 치는것처럼 다른 모듈과 객체로 변경에 대한 요구가 전파되지 않는 상태
- UserDao와 ConnectionMaker는 인터페이스를 통해 느슨하게 연결되어 있다.

###  

### (대체 가능한)전략 패턴

- 객체들의 행위에 대해 클래스로 정의하고 공통 행위에 대한 인터페이스를 정의하여 객체의 행위를 동적으로 변경하고 싶은 경우 행위를 수정하지 않아도 되고 전략(인터페이스를 상속받아 각 행위를 정의한 각 클래스)을 바꿔줌으로써 행위의 수정이 가능하게 고안된 패턴
- Client(UserDaoTest) - UserDao - ConnectionMaker
- Client가 ConnectionMaker을 변경해서 UserDao의 동작을 수정한다





팩토리 클래스

-   오브젝트를 생성하는 쪽과 생성된 오브젝트를 사용하는 쪽의 역할과 책임을 깔끔하게 분리하려는 목적

**오브젝트 팩토리를 활용한 구조**

```
public class DaoFactory {
    public UserDao userDao{
        return new UserDao(connectionMaker());
    }

    public AccountDao accountDao(){
        return new AccountDao(connectionMaker());
    }

    public MessageDao messageDao(){
        return new MessageDao(connectionMaker());
    }

    public ConnectionMaker connectionMaker(){
        return new DConnectionMaker();
    }
}

public class UserDaoTest {
    public static void main(String[] args) throws ClassNotFoundException, SQLException{
        UserDao dao = new DaoFactory().userDao();

    }
}
```

-   애플리케이션의 컴포넌트 역할을 하는 오브젝트와 애플리케이션의 구조를 결정하는 오브젝트 분리

**제어권의 이전을 통한 제어관계 역전**

-   제어의 역전이란 일반적인 제어 흐름(자신이 사용할 클래스를 자신이 결정하고 오브젝트를 직접 생성)의 개념을 거꾸로 뒤집는 것
-   서블릿을 생각하면 서블릿에 대한 제어 권한을 가진 컨테이너가 적절한 시점에 서블릿 클래스의 오브젝트를 만들고 그 안의 메서드를 호출
-   템플릿 메소드 패턴, 프레임워크, DaoFactory(해당 메소드가 전달해주는 connectionMaker를 사용해야함)

**스프링이 제공하는 IoC**

-   스프링이 제어권을 가지고 직접 만들과 관계를 부여하는 오브젝트를 빈
    -   오브젝트 단위의 애플리케이션 컴포넌트
    -   제어의 역전이 적용된 오브젝트
-   빈 팩토리(애플리케이션 컨텍스트)
    -   빈의 생성과 관계설정같은 제어를 담당하는 IoC 오브젝트
    -   별도의 정보를 참고해서 빈의 생성과 관계설정

Bean을 등록하고 애플리케이션 컨택스트 getBean()

```
@Configuration //빈 팩토리가 사용할 설정 정보
public class DaoFactory {

    @Bean //IoC용 메소드라는 표시
    public UserDao userDao{
        return new UserDao(connectionMaker());
    }

    @Bean
    public ConnectionMaker connectionMaker(){
        return new DConnectionMaker();
    }
}

public class UserDaoTest {
    public static void main(String[] args) throws ClassNotFoundException, SQLException{
        ApplicationContext context = new AnnotationConfigApplicationContext(DaoFactory.class);
        UserDao dao = context.getBean("userDao", UserDao.class); //두번째 파라미터는 캐스팅용

    }
}
```

**애플리케이션 컨텍스트의 동작방식**

-   ApplicationContext가 BeanFactory 인터페이스를 상속한 형태
-   @Configuration이 붙은 클래스를 설정정보로 등록해 두고 @Bean이 붙은 메소드의 이름을 가져와 빈 목록을 구성
-   클라이언트는 구체적인 Factory 클래스를 알 필요가 없음
-   전처리, 후처리, 정보의 조합, 설정방식 다양화 등 여러가지 기능제공
-   다양한 빈 검색 방법 제공

**싱글톤 레지스트리와 오브젝트 스코프**

-   getBean()으로 가져오는 오브젝트는 동일한 오브젝트가 반환된다
-   기본적으로 스프링은 내부에서 생성하는 빈을 싱글톤으로 생성한다
-   매번 요청이 들어올때마다 각 로직을 담당하는 오브젝트를 새로 만든다면?
-   서블릿은 서블릿 클래스당 하나의 오브젝트만 만들어두고, 사용자의 요청을 담당하는 여러 스레드에서 하나의 오브젝트를 공유해 동시에 사용

**싱글톤 패턴 UserDao**

```
public class UserDao {
    private static UserDao INSTANCE;
    private ConnectionMaker connectionMaker;

    public UserDao(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }

    public static synchronized UserDao getInstance(){
        if(INSTANCE == null) INSTANCE = new UserDao();
        return INSTANCE;
    }
}
```

-   private 생성자가 있으면 상속을 할 수 없다
-   싱글톤은 테스트가 힘들다(오브젝트 대체가 힘듬)
-   여러개의 JVM인 경우? 하나만 만들어지는 것을 보장하지 못한다
-   무조건적인 전역상태는 바람직하지 못하다

**스프링의 싱글톤 레지스트리**

-   평범한 자바 클래스를 싱글톤 형태로 활용하게 해 준다
-   public이지만 싱글톤으로 활용가능, 테스트를 위한 목 오브젝트 대체도 가능
-   생성자 파라미터로 오브젝트도 넣어줄 수 있다
-   멀티 스레드 환경에서는 무상태 방식으로 만들어야함
-   경우에 따라 스코프를 달리 가져갈 수 있다



### 의존관계 주입

-   스프링 IoC 기능의 대표적인 동작 원리는 주로 의존관계 주입
-   의존하고 있다는 것은 의존대상이 변하면 의존하는것에도 영향을 준다는 것
-   방향성이 있어서 반대로는 적용되지 않음
-   인터페이스를 사용해서 느슨한 의존관계를 갖는 경우 런타임에 사용할 오브젝트가 어떤 클래스로 만든 것인지 알 수 없다
-   런타임 시에 의존관계를 갖는 클라이언트와 의존 오브젝트를 연결해 주는것이 의존관계 주입
-   코드상에 런타임 시의 의존관계가 미리 결정되어 있는것은 좋지 않다
-   앞의 예제에서 DaoFactory 가 런타임시 오브젝트간의 의존관계를 설정해 준다

### 의존관계 검색

-   런타임 시의 필요한 의존관계를 스스로 컨테이너에 요청
-   getBean()
-   코드가 지저분해진다, 스프링이나 오브젝트 팩토리를 만들고 이용하는 코드가 섞임
-   의존관계 주입과 다르게 사용하는 오브젝트가 스프링의 빈일 필요가 없다
    -   의존관계를 주입받고 싶으면 자기 자신이 컨테이너가 관리하는 빈이 돼어야 한다

**기능 구현의 교환**

```
//Local DB, 운영용DB전환, DaoFactory에서 간단하게 수정하면 끝
@Configuration
public class DaoFactory {

    @Bean
    public UserDao CustomUserDao{
        return new UserDao(connectionMaker());
    }

    @Bean
    public ConnectionMaker connectionMaker(){
        return new DConnectionMaker();
    }

    @Bean
    public ConnectionMaker connectionMaker(){
        return new LocalConnectionMaker();
    }
}
```

**부가 기능 추가**

-   기존 ConnectionMaker를 상속하면서 connection을 반환하는거 와 추가로 부가 기능을 삽입

```
public class CountingConnectionMaker implements ConnectionMaker {
    int counter = 0;
    private ConnectionMaker realConnectionMaker;

    public CountingConnectionMaker(ConnectionMaker realConnectionMaker) {
        this.realConnectionMaker = realConnectionMaker;
    }

    public Connection makeConnection() throws ClassNotFoundException, SQLException{
        this.counter++;
        return realConnectionMaker.makeConnection();
    }

    public int getCounter() {
        return counter;
    }
}
```

**Setter를 이용한 DI**

```
public class UserDao {
    private ConnectionMaker connectionMaker;

    public void setConnectionMaker(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }
}
```

**XML을 사용한 Bean등록**

-   빈의 이름 id옵션, 빈의 클래스 class옵션, 빈의 의존 오브젝트 property 옵션(name 프로퍼티의 이름, ref 주입해줄 빈 이름)

```
<beans>
    <bean id="myConnectionMaker" class="package.DConnectionMaker"/>
    <bean id="userDao" class="package.UserDao">
        <property name="connectionMaker" ref="connectionMaker"/> 
         // new connectionMaker() 를 사용해서 connectionMaker를 주입 하라는 뜻
    </bean>
</beans>
```

applicationContext.xml에 bean을 등록하고 다음과 같이 설정을 가져올 수 있다.

```
ApplicationContext context = new GenericXmlApplicationContext(applicationContext.xml);
```

**DataSource를 사용하도록 변경**

```
public class UserDao {

    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void add(User user) throws ClassNotFoundException, SQLException{
        Connection c = dataSource.getConnection();
    }

    public User get(String id) throws ClassNotFoundException, SQLException{
        Connection c = dataSource.getConnection();
    }
}

@Configuration
public class DaoFactory {

    @Bean
    public DataSource dataSource(){
        SimpleDriverDataSource dataSource = new SimpleDriverDataSource();

        dataSource.setDriverClass(com.mysql.jdbc.Driver.class);
        dataSource.setUrl("jdbc:mysql://localhost/springbook");
        dataSource.setUsername("root");
        dataSource.setPassword("********");

        return dataSource;
    }

    @Bean
    public UserDao userDao(){
        UserDao userDao = new UserDao();
        userDao.setDataSource(dataSource());
        return userDao();
    }
}
```

