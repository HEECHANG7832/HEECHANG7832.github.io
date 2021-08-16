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
