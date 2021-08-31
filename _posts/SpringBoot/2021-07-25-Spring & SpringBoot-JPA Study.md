---
layout: post
title: "JPA Study"
categories:
  - SpringBoot
tags:
  - SpringBoot
---


### JPA


ORM Object Related Mapping

- 객체와 데이터베이스의 연결

JAVA Persistence API - Persistence영역을 다루기 위한 규격 정의

Hibernate - JPA의 규격을 따르는 구현체

Spring Data Jpa - Hibernate를 좀더 추상화 해 둔것



프로젝트 생성할때

Spring Data Jpa, H2 DB 선택

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    runtimeOnly 'mysql:mysql-connector-java'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

application.yml

```
spring:
  h2:
    console:
      enabled: true //h2 console enable
  jpa:
    show-sql: true //실행되는 쿼리 보기
    properties:
      hibernate:
        format_sql: true //쿼리 포맷 지정
```



JPA는 인자가 없는 생성자가 필수 - @NoArgConstructor

@RequiredArgConstructor - @NonNull인 생성자



##### @EqualsAndHashCode

##### equals, hashCode 자동 생성

- **equals** :  두 객체의 내용이 같은지, 동등성(equality) 를 비교하는 연산자
- **hashCode** : 두 객체가 같은 객체인지, 동일성(identity) 를 비교하는 연산자



자바 bean에서 동등성 비교를 위해 equals와 hashcode 메소드를 오버라이딩해서 사용하는데,

@**EqualsAndHashCode**어노테이션을 사용하면 자동으로 이 메소드를 생성할 수 있다.

callSuper 속성을 통해 eqauls와 hashCode 메소드 자동 생성 시 부모 클래스의 필드까지 감안할지의 여부를 설정할 수 있다.



@EqualsAndHashCode(callSuper = true)로 설정시 부모 클래스 필드 값들도 동일한지 체크하며, false(기본값)일 경우 자신 클래스의 필드 값만 고려한다.





### CH 02



@SpringBootTest - 스프링 컨텍스트를 테스트에 활용하겠다



**JPA활용**

```java
userRepository.save(new User());

userRepository.findAll();
```

그 외 함수들

**JpaRepository**

findAllById()

saveAll()

flush()

saveAndFlush()

deleteInBatch()

deleteAllInBatch(); - 검색하는 쿼리를 사용하지 않고 하나의 쿼리로 삭제, 테이블 전체 삭제

getOne(Id) - Id조회

findAll(example 또는 sort)



**PagingAndSortingRepository**

findAll(Pageable) - 페이징 처리를 지원



**CrudRepository**

save, saveAll, findById, existsById, deleteById(id), count(), delete(entity), deleteAll()



**QueryByExampleExecutor** - example을 활용한 query



**Repository** - 최상위 인터페이스, 내용없음

---

test resources에 data.sql  을 만들면 test전에 한번 쿼리를 실행시켜준다







```java
//페이지 지원
Page<User> users = userRepository.findAll(PageRequest.of(page , size));

//Example 지원
//검색이 필요한 인자를 Example로 만들어 쿼리 가능
ExampleMatcher matcher = ExampleMatcher.matching()
    .withIgnorePath("name")
    .withMatcher("email", endsWith());


Example<User> example = Example.of(new User("ma", "fastcampus.com"), matcher);

userRepository.findAll(example).forEach(System.out:println);

```



update를 구현할때

save()를 사용할때 기존 값을 변경하는 경우 즉 entity가 존재하는 경우 select 쿼리를 한번 작동한 뒤 update 쿼리를 사용한다

saveAll()을 사용하면 insert코드가 여러번 실행됨



### CH 03

**Query Method의 활용**

- JpaRepository에 추상메서드로 선언해서 사용할 수 있는 메서드

- 반환값은 다앙햔 형식을 지원한다

```java
Optional<User> findByName(String Name);
```



**Select구문**

findBy, getBy, readBy, queryBy, searchBy, streamBy, findUserByEmail, find***ByEmail, findTop1ByEmail, findFirst1ByEmail



**Query Method에 대해서는 테스트 를 작성하는게 좋다**



```java
public interface UserRepository extends JpaRepository<User, Long> {
    Set<User> findByName(String name);

    Set<User> findUserByNameIs(String name);
    Set<User> findUserByName(String name);
    Set<User> findUserByNameEquals(String name);

    User findByEmail(String email);

    User getByEmail(String email);

    User readByEmail(String email);

    User queryByEmail(String email);

    User searchByEmail(String email);

    User streamByEmail(String email);

    User findUserByEmail(String email);

    User findSomethingByEmail(String email);

    List<User> findFirst2ByName(String name);

    List<User> findTop2ByName(String name);

    List<User> findLast1ByName(String name);

    List<User> findByEmailAndName(String email, String name);

    List<User> findByEmailOrName(String email, String name);

    List<User> findByCreatedAtAfter(LocalDateTime yesterday);

    List<User> findByIdAfter(Long id);

    List<User> findByCreatedAtGreaterThan(LocalDateTime yesterday);

    List<User> findByCreatedAtGreaterThanEqual(LocalDateTime yesterday);

    List<User> findByCreatedAtBetween(LocalDateTime yesterday, LocalDateTime tomorrow);

    List<User> findByIdBetween(Long id1, Long id2);

    List<User> findByIdGreaterThanEqualAndIdLessThanEqual(Long id1, Long id2);

    List<User> findByIdIsNotNull();

//    List<User> findByAddressIsNotEmpty();   // name is not null and name != '' ??

    List<User> findByNameIn(List<String> names);

    List<User> findByNameStartingWith(String name);

    List<User> findByNameEndingWith(String name);

    List<User> findByNameContains(String name);

    List<User> findByNameLike(String name);

    List<User> findTop1ByName(String name);

    List<User> findTopByNameOrderByIdDesc(String name);

    List<User> findFirstByNameOrderByIdDescEmailAsc(String name);

    List<User> findFirstByName(String name, Sort sort);

    Page<User> findByName(String name, Pageable pageable);

    @Query(value = "select * from user limit 1;", nativeQuery = true)
    Map<String, Object> findRawRecord();

    @Query(value = "select * from user", nativeQuery = true)
    List<Map<String, Object>> findAllRawRecord();
}

    @Test
    void pagingAndSortingTest() {
        System.out.println("findTop1ByName : " + userRepository.findTop1ByName("martin"));
        System.out.println("findTopByNameOrderByIdDesc : " + userRepository.findTopByNameOrderByIdDesc("martin"));
        System.out.println("findFirstByNameOrderByIdDescEmailAsc : " + userRepository.findFirstByNameOrderByIdDescEmailAsc("martin"));
        System.out.println("findFirstByNameWithSortParams : " + userRepository.findFirstByName("martin", Sort.by(Order.desc("id"), Order.asc("email"))));
        System.out.println("findFirstByNameWithSortParams : " + userRepository.findFirstByName("martin", Sort.by(Order.desc("id"), Order.asc("email"))));
        System.out.println("findFirstByNameWithSortParams : " + userRepository.findFirstByName("martin", Sort.by(Order.desc("id"), Order.asc("email"))));
        System.out.println("findByNameWithPaging : " + userRepository.findByName("martin", PageRequest.of(1, 1, Sort.by(Order.desc("id")))).getTotalElements());
    }
```





### Entity 속성들

@GenerationType - Id값 설정 규칙 지정

@Table - table 의 이름이나 스키마를 직접 지정할때

@Column - 필드 속성 지정, 이름 지정 가능, nullable, unique

@Transient - 영속성 처리에서 제외됨 DB에서 제외되고 객체에서 값으로 존재

Enum값을 넣을때 Enum값 숫자 그대로 들어간다 @Enumarated를 붙이면 ENUM 값을 스트링으로 그대로 가져감



### Entity event 함수들

prePersist()

postPersist()

preUpdate()

postUpdate()

preRemove()

postRemove()

시간을 column에 추가한다던지 기본값들 지정할때 사용



@EntityListener(value = MyEntityListener.class)

- Custom한 EneityListtener를 만들 수 있다

수정된 내용의 히스토리가 필요한 경우

```java
public class UserEntityListener {
    @PostPersist
    @PostUpdate
    public void prePersistAndPreUpdate(Object o) {
        UserHistoryRepository userHistoryRepository = BeanUtils.getBean(UserHistoryRepository.class);

        User user = (User) o;

        UserHistory userHistory = new UserHistory();
        userHistory.setName(user.getName());
        userHistory.setEmail(user.getEmail());
        userHistory.setUser(user);
        userHistory.setHomeAddress(user.getHomeAddress());
        userHistory.setCompanyAddress(user.getCompanyAddress());

        userHistoryRepository.save(userHistory);
    }
}
```





AuditingEntityListener를 추가하고

@CreatedDate

​	private String createdAt;

@LastModifiedDate

사용하면 JPA에서 제공하는 auditing을 지원받을 수 있다

@MappedSuperClass



### 연관관계



primitive type이 아니라면 null check를 해줘야함 primitive type은 자동으로 not null지정됨



**1대 1관계**

Book - BookReviewInfo

```
//Book.class
@OneToOne(mappedBy = "book") //book에서는 bookReviewInfo id가 없게
@ToString.Exclude
private BookReviewInfo bookReviewInfo;

//BookReviewInfo.class
@OneToOne(optional = false)
private Book book;
```

북 리뷰의 id를 통해서 같은 id를 가지는 book을 가져올수 있고 반대로도 된다



Optional 반환값의 경우 orElseThrow를 사용해야함



strategy를 AUTO로 사용해야지 hibernate_sequence를 사용한다

IDENTITY는 mysql의 기본값으로 각 table마다 id를 각자 증가시킨다



코드 주석은 삭제하는게 좋다



기본적으로 RDB에서는 연관된 table의 id FK로 사용해서 쿼리하는데 JPA에는 다른 방식 제공

@OneToOne을 선언하면 알아서 객체의 ID 컬럼을 만들어서 연관성 설정한다

mappedby 속성 - 연관키를 해당 테이블에서 가지고 있지 않게 된다

롬복 tostring 에서 무한 참조가 발생하기 때문에 릴레이션은 단방향으로 걸거나 toString을 제외해야한다





**1대 N 관계**

User - UserHistory

기본 리스트의 경우에는 new ArrayList<>(); 로 기본 생성자를 생성해주는 것이 좋다

```java
//User.class
@OneToMany(fetch = FetchType.EAGER) //Transaction 관련
@JoinColumn(name = "user_id", insertable = false, updatable = false) //불필요한 중간 Entity가 생기는 경우 join column을 user_id로 정해주고 수정과 저장이 안되게 설정
@ToString.Exclude
private List<UserHistory> userHistories = new ArrayList<>();

//UseHistory.class
@Column(name = "user_id")
private Long userId;
```



**N대 1관계**

UserHistory - User

```java
@ManyToOne
@ToString.Exclude
private User user; //user_id를 생성해서 사용한다
```



Entity에서 필요한 연관관계를 설정해 준다

User에서 UserHistory를 조회하는게 자연스러움



**양방향 관계**

Review

```java
//User.class
@OneToMany
@JoinColumn(name = "user_id") //중간 table제거
@ToString.Exclude
private List<Review> reviews = new ArrayList<>();

//Book.class
@OneToMany
@JoinColumn(name = "book_id")
@ToString.Exclude
private List<Review> reviews = new ArrayList<>();

//Review.class
@ManyToOne(fetch = FetchType.LAZY)
@ToString.Exclude
private User user;

@ManyToOne(fetch = FetchType.LAZY)
@ToString.Exclude
private Book book;
```



Publisher

```java
//Publisher.class
@OneToMany(orphanRemoval = true)
@JoinColumn(name = "publisher_id")
@ToString.Exclude
private List<Book> books = new ArrayList<>();

//Book.class
@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE }) @ToString.Exclude
private Publisher publisher;
```

