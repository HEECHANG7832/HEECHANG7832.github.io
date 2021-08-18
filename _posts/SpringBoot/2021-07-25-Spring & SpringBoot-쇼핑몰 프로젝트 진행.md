---
layout: post
title: "쇼핑몰 프로젝트 진행"
categories:
  - SpringBoot
tags:
  - SpringBoot
---


UUID?

```java
    @Id
    @GeneratedValue(generator = "UUID")
    @GenericGenerator(name = "UUID", strategy = "org.hibernate.id.UUIDGenerator")
    @Column(columnDefinition = "BINARY(16)")
    private UUID id;
```



BFS

```java
public static void bfs_list(int v, LinkedList<Integer>[] adjList, boolean[] visited) {
	Queue<Integer> queue = new LinkedList<Integer>();
	visited[v] = true;
	queue.add(v);

	while(queue.size() != 0) {
		v = queue.poll();
		System.out.print(v + " ");

		Iterator<Integer> iter = adjList[v].listIterator();
		while(iter.hasNext()) {
			int w = iter.next();
			if(!visited[w]) {
				visited[w] = true;
				queue.add(w);
			}
		}
	}
}
```





build.gradle에서 implementation과 compile 의 차이


AOP에서 값을 꼭 돌려줘야 된다






```java
BindingResult bindingResult

         if (bindingResult.hasErrors()){
            String errorMessage = bindingResult.getAllErrors().get(0).getDefaultMessage();
            return new ResponseEntity<>(errorMessage, HttpStatus.BAD_REQUEST);
        }
```



csrf ??

RestController와 Controller선언의 차이점



## 끝판왕! @Data

`@Data`는 위에서 설명드린 `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@ToString`, `@EqualsAndHashCode`을 한꺼번에 설정해주는 매우 유용한 어노테이션입니다.

```java
@Data
public class User {
  // ...
}
```

사용 방법은 다른 어노테이션들과 대동소이합니다. 클래스 레벨에서 `@Data` 어노테이션을 붙여주면, 모든 필드를 대상으로 접근자와 설정자가 자동으로 생성되고, `final` 또는 `@NonNull` 필드 값을 파라미터로 받는 생성자가 만들어지며, `toStirng`, `equals`, `hashCode` 메소드가 자동으로 만들어집니다.



**빈 주입 안됨**

```java
@Slf4j
@RequiredArgsConstructor
@RestController
@RequestMapping("/api")
public class ProductRestController {
	//private ProductService productService; //bean 주입 안됨
    private final ProductService productService; //bean 주입 됨
}
```



schema 에 있는 스크립트를 스프링부트 실행시마다 실행시키면 테이블이 중복돼서 에러가 발생한다.

그래서 디비 초기화는 처음 한번만 하고 그다음부터는 never로 바꿔야 한다.



spring.datasource.initialization-mode=never



->spring.datasource.initialization-mode=always



### Data.sql은 왜 안될까

    hibernate:
      ddl-auto: none #!!!!! 이거때문에 다시 만드는듯..
	  
	  확인 필요

    






    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.security:spring-security-test'

```java
package com.example.springbootshoppingmallproject.config;


import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.security.servlet.PathRequest;
import org.springframework.boot.web.servlet.ServletListenerRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.access.hierarchicalroles.RoleHierarchy;
import org.springframework.security.access.hierarchicalroles.RoleHierarchyImpl;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.session.SessionRegistry;
import org.springframework.security.core.session.SessionRegistryImpl;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.LoginUrlAuthenticationEntryPoint;
import org.springframework.security.web.authentication.rememberme.JdbcTokenRepositoryImpl;
import org.springframework.security.web.authentication.rememberme.PersistentTokenRepository;
import org.springframework.security.web.session.HttpSessionEventPublisher;

import javax.sql.DataSource;

@Slf4j
@RequiredArgsConstructor
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //private final CustomUserDetailsService customUserDetailsService;
    private final DataSource dataSource;


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
                        request.antMatchers("/").permitAll()
                                //.anyRequest().authenticated()
                )
                .formLogin(login->
                        login.loginPage("/login")
                                .loginProcessingUrl("/loginprocess")
                                .permitAll()
                                .defaultSuccessUrl("/", false)
                                //.authenticationDetailsSource(customAuthDetail)
                                .failureUrl("/login-error")
                )
                .logout(logout->
                        logout.logoutSuccessUrl("/"))
                .exceptionHandling(error->
                        error.accessDeniedPage("/access-denied")
                )
        ;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring()
                .requestMatchers(PathRequest.toStaticResources().atCommonLocations())
        ;
    }
}

```