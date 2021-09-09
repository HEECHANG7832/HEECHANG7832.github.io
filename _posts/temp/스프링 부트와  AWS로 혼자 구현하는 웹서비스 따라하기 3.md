



### 스프링 시큐리티를 활용한 Oauth2클라이언트





> spring-security-oauth2-autoconfigure

스프링 1.5에서 쓰던 설정 그대로 2.0에서 사용할 수 있게 해준다

- 2.0은 url을 명시하지 않아도 되고 client 인증 정보만 입력하면 된다.
- CommonOauth2Provider라는 enum이 추가되어 구글, 깃허브 등의 설정값이 제공됨
-  기본적으로 {도메인}/login/oauth2/code/{소셜서비스코드} 로 리다이렉트 URL을 제공





**User.java**

```java
@Getter
@NoArgsConstructor
@Entity
public class User extends BaseTimeEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private String email;

    @Column
    private String picture;

    //Enum값을 int형이 아닌 문자열로 저장
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;

    @Builder
    public User(String name, String email, String picture, Role role) {
        this.name = name;
        this.email = email;
        this.picture = picture;
        this.role = role;
    }

    public User update(String name, String picture) {
        this.name = name;
        this.picture = picture;

        return this;
    }

    public String getRoleKey() {
        return this.role.getKey();
    }
}
```





**User의 CRUD를 담당할 UserRepository 작성**

```java
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email); //소셜 로그인으로 반환되는 값중 email을 통해 이미 생성된 사용자인지 판단
}
```





**Role.java**

```java
package com.jojoldu.book.springboot.domain.user;

import lombok.Getter;
import lombok.RequiredArgsConstructor;

@Getter
@RequiredArgsConstructor
public enum Role {

    //스프링 시큐리티에서는 권한코드 앞에 ROLE_이 있어야함
    GUEST("ROLE_GUEST", "손님"),
    USER("ROLE_USER", "일반 사용자");

    private final String key;
    private final String title;

}

```





**SecurityConfig.java**

```java
@RequiredArgsConstructor
@EnableWebSecurity //Spring Security 설정 활성화
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .headers().frameOptions().disable() //h2-console 화면을 사용하기 위해 해당 옵션들 disable
                .and()
                    .authorizeRequests() //URL별 옵션 관리를 위한 시작점, antMatchers 옵션사용 가능
                    .antMatchers("/", "/css/**", "/images/**", "/js/**", "/h2-console/**", "/profile").permitAll() //URL별, HTTP메소드별 권한 관리 대상을 지정, permitAll()옵션으로 전체 열람권한 부여
                    .antMatchers("/api/v1/**").hasRole(Role.USER.name()) //USER권한을 가진 사람만 가능
                    .anyRequest().authenticated() //나머지 URL은 모두 인증된 사용자들만 허용
                .and()
                    .logout()
                        .logoutSuccessUrl("/") //로그아웃 성공시 "/" 주소로 이동
                .and()
                    .oauth2Login() //로그인 성공시 가져올 정보
                        .userInfoEndpoint() 
                            .userService(customOAuth2UserService); //소셜 로그인 성공 시 후속 조치를 진행할 UserService 인터페이스의 구현체를 등록, 사용자 정보를 가져온 상태에서 추가로 진행하고자 하는 기능 구현가능
    }
}
```





CustomOauth2UserService.java

```java
@RequiredArgsConstructor
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
    private final UserRepository userRepository;
    private final HttpSession httpSession;

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        OAuth2UserService delegate = new DefaultOAuth2UserService();
        OAuth2User oAuth2User = delegate.loadUser(userRequest);
	
        //현재 로그인 진행중인 소셜 서비스 구분
        String registrationId = userRequest.getClientRegistration().getRegistrationId();
        //Oauth2 로그인 진행시 키가 되는 필드값
        String userNameAttributeName = userRequest.getClientRegistration().getProviderDetails()
                .getUserInfoEndpoint().getUserNameAttributeName();

        //OAuth2User service에서 attribute를 담을 클래스
        OAuthAttributes attributes = OAuthAttributes.of(registrationId, userNameAttributeName, oAuth2User.getAttributes());

        User user = saveOrUpdate(attributes);
        
        //SessionUser 세션에 사용자 정보를 저장하기 위한 Dto class
        httpSession.setAttribute("user", new SessionUser(user));

        return new DefaultOAuth2User(
                Collections.singleton(new SimpleGrantedAuthority(user.getRoleKey())),
                attributes.getAttributes(),
                attributes.getNameAttributeKey());
    }


    private User saveOrUpdate(OAuthAttributes attributes) {
        User user = userRepository.findByEmail(attributes.getEmail())
                .map(entity -> entity.update(attributes.getName(), attributes.getPicture()))
                .orElse(attributes.toEntity());

        return userRepository.save(user);
    }
}
```





**OAuthAttributes.java**

```java
package com.jojoldu.book.springboot.config.auth.dto;

import com.jojoldu.book.springboot.domain.user.Role;
import com.jojoldu.book.springboot.domain.user.User;
import lombok.Builder;
import lombok.Getter;

import java.util.Map;

@Getter
public class OAuthAttributes {
    private Map<String, Object> attributes;
    private String nameAttributeKey;
    private String name;
    private String email;
    private String picture;

    @Builder
    public OAuthAttributes(Map<String, Object> attributes, String nameAttributeKey, String name, String email, String picture) {
        this.attributes = attributes;
        this.nameAttributeKey = nameAttributeKey;
        this.name = name;
        this.email = email;
        this.picture = picture;
    }
	
    //OAuth2User에서 반환하는 사용자 정보는 Map이기 때문에 of를 사용해서 하나하나 변환한다
    public static OAuthAttributes of(String registrationId, String userNameAttributeName, Map<String, Object> attributes) {
        if("naver".equals(registrationId)) {
            return ofNaver("id", attributes);
        }

        return ofGoogle(userNameAttributeName, attributes);
    }

    private static OAuthAttributes ofGoogle(String userNameAttributeName, Map<String, Object> attributes) {
        return OAuthAttributes.builder()
                .name((String) attributes.get("name"))
                .email((String) attributes.get("email"))
                .picture((String) attributes.get("picture"))
                .attributes(attributes)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    private static OAuthAttributes ofNaver(String userNameAttributeName, Map<String, Object> attributes) {
        Map<String, Object> response = (Map<String, Object>) attributes.get("response");

        return OAuthAttributes.builder()
                .name((String) response.get("name"))
                .email((String) response.get("email"))
                .picture((String) response.get("profile_image"))
                .attributes(response)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    //User Entity를 생성
    public User toEntity() {
        return User.builder()
                .name(name)
                .email(email)
                .picture(picture)
                .role(Role.GUEST)
                .build();
    }
}
```





SessionUser.java

```java
@Getter
public class SessionUser implements Serializable {
    private String name;
    private String email;
    private String picture;

    public SessionUser(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.picture = user.getPicture();
    }
}
```







User 클래스를 세션에 바로 저장하면 빌드 에러가 난다

- 직렬화를 구현하라고 하는데 Entity클래스는 언제든 다른 엔티티와 관계가 형성될 수 있기 때문에 직렬화 코드를 넣기가 부자연스럽다
- 따라서 직렬화 기능을 구현한 Dto클래스를 하나 추가로 만든 것





로그인 버튼 구현

- a href="/logout"
  - 스프링 시큐리티에서 기본적으로 제공하는 로그아웃 URL, 별도의 컨트롤러를 생성할 필요가 없다
- a href="/oauth2/authorization/google"
  - 기본 제공하는 로그인 URL





Session 저장소를 DB로 설정

- build.gradle 에 spring-session-jdbc를 추가
- application.properties에 spring.session.store-type=jdbc 설정
- JPA에 의해 SPRING_SESSION, SPRING_SESSION_ATTRIBUTES 테이블이 자동 생성된다





**기존 Test 에 시큐리티 적용**

- src/main, src/test는 환경이 다르기 때문에 src/main에 설정한 apllication.properties는 자동으로 가져오지만 application-oauth.properties는 가져오지 않는다
- application.properties에 가짜 설정값을 등록해서 사용한다
- 스프링 시큐리티 테스트를 위한 도구 spring-security-test를 추가
  - @WithMockUser(roles="USER")
  - 인증된 모의 사용자를 만들어서 사용, roles에 권한을 추가한다
  - ROLE_USER 권한을 가진 사용자가 api를 호출하는 것과 같은 효과





### AWS 서버 구축



EC2 인스턴스 구축 - Elastic Compute Cloud



EC2 - t2.micro 선택, 1년무료

AMI(Amazon Machine Image) 이미지 선택 - 인스턴스를 시작하는 데 필요한 정보를 이미지로 만들어 둔 것

* 현재는 아마존 리눅스 1은 없고 2를 선택
* 레드햇 베이스
* Amazon 독자적인 개발 리포지터리 사용으로 yum이 매우 빠르다



EC2 태그 (키-값) 설정

EC2 보안 그룹(방화벽) 생성 - 자신의 ip

- pem 키 관리와 지정된 ip 에서 ssh접속이 가능하도록 해야함



인스턴스에 접속하기 위한 pem 키 생성

인스턴스가 생성되면 IP와 도메인이 할당된다

- 인스턴스를 중지하고 다시 실행해도 IP가 새로 할당되기 때문에 고정 IP 할당해야함(EIP)
- EIP는 사용하지 않을때 삭제하자, 비용청구될 수 있음



**Window 에서 putty로 접속**

- pem키를 ppk파일로 변환(puttygen)
- username@public_ip
  - ec2-user@생성한 ip
  - ppk 파일 지정



**설치후 해야할 것**

java 설치

 $sudo yum install -y java-1.8.0-openjdk-devel.x86_64

$ sudo /usr/sbin/alternatives --config java

$ java -version

시간변경

$sudo rm /etc/localtime

$sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

hostname 수정

$sudo vim /etc/sysconfig/network





### AWS RDS

**MariaDB**

- 프리티어 무료, MySQL 기반으로 만들어짐
- 동일 하드웨어 사향으로 MySQL보다 향상
- 커뮤니티, 다양한 스토리지 엔진



**RDS 환경설정**

파라미터 그룹에 세팅하고 RDS에 적용

- 타임존
- Character set
  - 이모지 저장이 가능한 utf8mb4 사용
- Max Connection



**RDS에 접속하기**

- EC2에 사용된 보안 그룹 ID를 인바운드로 추가
  - EC2와 RDS간 접근을 위해
- Intellij Database 플러그인 설치
- RDS 엔드포인트 확인

```
//database 선택
use springboot2_webservice;

//현재의 character_set, collation 확인
show variables like 'c%';

//파라미터 변경
ALTER DATABASE springboot2_webservice
CHARACTER SET = 'utf8mb4'
COLLATE = 'utf8mb4_general_ci';

//시간 확인
SELECT @@time_zone, now();

//예제 쿼리
CREATE TABLE test(
    id bigint(20) NOT NULL AUTO_INCREMENT,
    content varchar(255) DEFAULT NULL,
    PRIMARY KEY (id)
)ENGINE=InnoDB;

insert into test(content) values ('테스트');

select * from test;
```



**EC2에서 RDS접근 확인**

$sudo yum install mysql

$mysql -u admin -p -h  RDS엔드포인트



### EC2서버에 프로젝트 배포하기



**git 설치및 클론**

$sudo yum install git

git --version

mkdir ~/app && mkdir ~/app/step1

cd ~/app/step1

git clone 주소



**테스트 실행하기**

./gradlew test



**gradlew 실행권한 변경하기**

chmod  +x ./gradlew



vim ~/app/step1/deploy.sh

```
#!/bin/bash

REPOSITORY=/home/ec2-user/app/step1
PROJECT_NAME=freelec-springboot2-webservice

cd $REPOSITORY/$PROJECT_NAME/

#변경사항 pull
echo "> Git Pull"
git pull

#빌드
echo "> 프로잭트 Build 시작"
 ./gradlew build

echo "> step1 디렉토리로 이동"
cd $REPOSITORY

echo "> Build  파일 복사"
cp $REPOSITORY/$PROJECT_NAME/build/libs/*.jar $REPOSITORY/

#실행중인 springboot 애플리케이션 확인해서 종료후 실행
echo "> 현재 구동중인 애플리케이션 pid확인"
CURRENT_PID=$(pgrep -f ${PROJECT_NAME}.*.jar)

echo "현재 구동중인 애플리케이션 pid: $CURRENT_PID"

if [ -z "$CURRENT_PID" ]; then
        echo "> 현재 구동중인 애플리케이션이 없음"
else
        echo "> kill -15 $CURRENT_PID"
        kill -15 $CURRENT_PID
        sleep 5
fi

echo "> 새 애플리케이션 배포"
JAR_NAME=$(ls -tr $REPOSITORY/ | grep jar | tail -n 1)

echo "> JAR Name: $JAR_NAME"

#jar 파일을 nohup 으로 실행
#스프링부트는 내장 톰캣을 가지고 있어서 jar파일로 바로 WAS를 실행할 수 있다
#java -jar을 사용하면 터미널 접속을 끊을때 같이 종료됨
nohup java -jar $REPOSITORY/$JAR_NAME 2>&1 &

#외부 Security 파일 등록
#client id같은 security정보는 git에 올려서 받을 수 없기 때문에
#직접 등록하여 사용
nohup java -jar \
	-Dspring.config.location=classpath:/application.properties,/home/ec2-user/app/application-oauth.properties \
	$REPOSITORY/$JAR_NAME 2>&1 &

#DB 설정파일 등록 추가
nohup java -jar \
	-Dspring.config.location=classpath:/application.properties,/home/ec2-user/app/application-oauth.properties,/home/ec2-user/app/application-real-db.properties,classpath:/application-real.properties \
	-Dspring.profiles.active=real \
	$REPOSITORY/$JAR_NAME 2>&1 &
  
```



nohup.out파일을 통해 로그를 확인 할 수 있다



**프로젝트에서 RDS접근하기**

- 테이블 생성

  - JPA가 사용할 Entity 테이블

  - 스프링 세션이 사용될  테이블

    ```
    #테스트 로그에서 확인가능
    Hibernate: create table posts (id bigint not null auto_increment, created_date datetime, modified_date datetime, author varchar(255), content TEXT not null, title varchar(500) not null, primary key (id)) engine=InnoDB
    Hibernate: create table user (id bigint not null auto_increment, created_date datetime, modified_date datetime, email varchar(255) not null, name varchar(255) not null, picture varchar(255), role varchar(255) not null, primary key (id)) engine=InnoDB
    
    #schema-mysql.sql
    CREATE TABLE SPRING_SESSION (
    	PRIMARY_ID CHAR(36) NOT NULL,
    	SESSION_ID CHAR(36) NOT NULL,
    	CREATION_TIME BIGINT NOT NULL,
    	LAST_ACCESS_TIME BIGINT NOT NULL,
    	MAX_INACTIVE_INTERVAL INT NOT NULL,
    	EXPIRY_TIME BIGINT NOT NULL,
    	PRINCIPAL_NAME VARCHAR(100),
    	CONSTRAINT SPRING_SESSION_PK PRIMARY KEY (PRIMARY_ID)
    ) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
    
    CREATE UNIQUE INDEX SPRING_SESSION_IX1 ON SPRING_SESSION (SESSION_ID);
    CREATE INDEX SPRING_SESSION_IX2 ON SPRING_SESSION (EXPIRY_TIME);
    CREATE INDEX SPRING_SESSION_IX3 ON SPRING_SESSION (PRINCIPAL_NAME);
    
    CREATE TABLE SPRING_SESSION_ATTRIBUTES (
    	SESSION_PRIMARY_ID CHAR(36) NOT NULL,
    	ATTRIBUTE_NAME VARCHAR(200) NOT NULL,
    	ATTRIBUTE_BYTES BLOB NOT NULL,
    	CONSTRAINT SPRING_SESSION_ATTRIBUTES_PK PRIMARY KEY (SESSION_PRIMARY_ID, ATTRIBUTE_NAME),
    	CONSTRAINT SPRING_SESSION_ATTRIBUTES_FK FOREIGN KEY (SESSION_PRIMARY_ID) REFERENCES SPRING_SESSION(PRIMARY_ID) ON DELETE CASCADE
    ) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
    
    
    ```

    

- 프로젝트 생성 - 자바 프로젝트가 MariaDB에 접근하려면 드라이버 필요

  - build.gradle에 mariadb 등록

- EC2 설정

  - vim ~/app/application-real-db.properties

    ```
    spring.jpa.hibernate.ddl-auto=none #테이블이 자동 생성되는 옵션None
    #RDS는 실제 운영이 되니 스프링 부트에서 새로 만들지 않게 해야함
    spring.datasource.url=jdbc:mariadb://RDS 주소:3306/springboot2_webservice
    spring.datasource.username=admin
    spring.datasource.password=
    spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
    
    ```

    

### CI & CD

CI - 지속적 톡합, 안정적인 배포 파일을 만드는 과정

CD - 지속적인 배포, 빌드 결과를 자동으로 운영 서버에 무중단 배포까지 진행



- Travics CI에서 빌드를 하고 S3에 jar파일을 넘긴다

- 이후 진행할 CodeDeploy는 저장 기능이 없기 때문에 배포와 빌드를 분리하기 위해 S3라는 중간 파일 서버를 둔다
- Travis CI가 S3와 CodeDeploy에 접근할 수 있게 AWS Key 발급, IAM



Travis CI 설정및 AWS S3와 연동

```
language: java
jdk:
  - openjdk8

branches:
  only:
    - master  #master branch 가 push 될때 빌드

# Travis CI 서버의 Home
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

script: "./gradlew clean build"

before_deploy: #deploy실행 전 실행
  - mkdir -p before-deploy # zip에 포함시킬 파일들을 담을 디렉토리 생성
  - cp scripts/*.sh before-deploy/
  - cp appspec.yml before-deploy/
  - cp build/libs/*.jar before-deploy/
  - cd before-deploy && zip -r before-deploy * # before-deploy로 이동후 전체 압축
  - cd ../ && mkdir -p deploy # 상위 디렉토리로 이동후 deploy 디렉토리 생성
  - mv before-deploy/before-deploy.zip deploy/freelec-springboot2-webservice.zip # deploy로 zip파일 이동

deploy: #s3등 외부 서비스와 연결될 행위 선언
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값
    bucket: freelec-springboot-build # S3 버킷
    region: ap-northeast-2
    skip_cleanup: true
    acl: private # zip 파일 접근을 private으로
    local_dir: deploy # before_deploy에서 생성한 디렉토리
    wait-until-deployed: true

  - provider: codedeploy
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값
    bucket: freelec-springboot-build # S3 버킷
    key: freelec-springboot2-webservice.zip # 빌드 파일을 압축해서 전달
    bundle_type: zip
    application: freelec-springboot2-webservice # 웹 콘솔에서 등록한 CodeDeploy 어플리케이션
    deployment_group: freelec-springboot2-webservice-group # 웹 콘솔에서 등록한 CodeDeploy 배포 그룹
    region: ap-northeast-2
    wait-until-deployed: true

# CI 실행 완료시 메일로 알람
notifications:
  email:
    recipients:
      - facee22av@naver.com

```



IAM 역활

- AWS 서비스에서만 할당할 수 있는 권한

사용자

- AWS 서비스 외에 사용할 수 있는 권한



**CodeDeploy**

- 배포 도구, 서버가 여러대일때 새로 빌드된 프로젝트를 일일히 모든 서버에 배포할 수 없다



EC2가 CodeDeploy에 연동할 수 있게 IAM 역활 생성후 인스턴스에 등록

### 

인스턴스에 CodeDeploy 에이전트 설치

$aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2

$chmod +x ./install

$sudo yum install ruby

$sudo ./install auto

$sudo service codedeploy-agent status



CodeDeploy에서 EC2접근하기 위한 IAM 역활 생성



Travis CI 에서 빌드가 끝나면 S3로 파일이 전송되고 이 파일은 EC2로 전송되어 압축이 풀어진다

- Travis CI의 설정 .travis.yml
- AWS CodeDeploy 설정 appspec.yml

**appspec.yml**

- Source : 루트 경로를 지정하면 전체 파일을 이동시킴
- Destination: source에서 지정된 파일을 받을 위치 옮겨진 jar이 이 파일들로 실행됨



CodeDeploy 에러 로그 확인  /opt/codedeploy-agent/deployment-root





### 무중단 서비스 만들기 - Nginx

웹 서버, 리버스 프록시, 캐싱, 로드밸런싱 등 지원하는 오픈소스 소프트웨어

무중단 배포에 사용되는 리버스 프록시



**엔진엑스 설치**

$ sudo yum install nginx

$ sudo amazon-linux-extras install nginx1

- 포트번호 80을 보안그룹에 추가

- Redirect URL 에서 8080 제거해서 추가
- 도메인 /login/oauth2/code/google

 

**엔진엑스 재 실행시**

sudo service nginx start

sudo systemctl start nginx
sudo systemctl status nginx



**엔진엑스 설정파일**

sudo vim /etc/nginx/nginx.conf

```
location / {

​	proxy_pass http://localhost:8080; #엔진엑스의 요청을 localhost로 전달

​	proxy_set_header X-Real-IP $remote_addr;

​	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

​	proxy_set_header Host $http_host;

}
```





### 의문점

JPA 영속성 컨텍스트

@Transactional

생성자에 빌더 패턴 적용?

도메인 모델 정리

