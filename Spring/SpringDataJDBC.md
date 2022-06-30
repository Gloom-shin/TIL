# Spring Data JDBC
> 이 글을 보기전에, 데이터베이스의 JDBC.md 와  JDBC와JPA.md를 읽고오길 바란다.
> 또한, 스프링의 대표적인 데이터베이스 접근 기술    
> `Spring Data JDBC`: 비교적 최근에 릴리즈된 기술   
> `JPA` : 가장 많이 사용하는 기술     
>  `Spring Data JPA` : JPA를 보다 편리하게 사용하는 기술  
>  세가지를 앞으로 숙지해야한다.  
> 그중 먼저 `Spring Data JDBC`를 살펴보자

<br></br>

# Spring Data JDBC란?
- 이름에 JDBC를 가지고있지만,`Spring Data JDBC`는 객체 중심의 기술이다. 
- Spring Data JDBC 기술은 2018년에 1.0 버전이 처음 릴리즈 되었기 때문에 기술의 역사가 아직 짧은 편
- 애플리케이션의 규모가 상대적으로 크지 않고, 복잡하지 않을 경우에는 Spring Data JDBC가 뛰어난 생산성을 보여줄거라 기대한다.


# Spring Data JDBC를 사용해보기  
## 사전 준비
### 의존라이브러리 추가
```java
dependencies {
	...
	...
	implementation 'org.springframework.boot:spring-boot-starter-data-jdbc'
	runtimeOnly 'com.h2database:h2'
}
```
 - Spring Data JDBC를 사용하기 위해선 `spring-boot-starter-data-jdbc`을 추가해줘야 한다.
 - 그리고 데이터베이스에서 데이터를 관리할 것이므로 개발 환경에서 손쉽게 사용할 수 있는 H2(인메모리DB)도 추가해주자