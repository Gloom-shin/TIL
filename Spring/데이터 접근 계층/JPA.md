# JPA
 - JPA(Java Persistence API)
 - Java 진영에서 사용하는 ORM(Object-Relational Mapping) 기술의 표준 사양(또는 명세, Specification)이다
   - 즉, Java의 인터페이스로 사양이 정의 되어 있기 때문에, JPA라는 표준사양을 구현한 `구현체`는 따로 있다.
 

## Hibernate ORM(구현체)
 - JPA표준 사양을 구현한 구현체로는 `Hibernate ORM`, `EclipseLink`, `DataNucleus` 등이 있다.
   - 그중  Hibernate ORM을 다룬다. 
   - Hibernate는 JPA에서 지원하는 기능외에도 자체적으로 사용할 수 있는 API도 지원한다.

> PA는 Java Persistence API의 약자이지만 현재는 [Jakarta Persistence](#jakartaPersistence)라고도 불린다.

<br></br>

## JPA 위치

<img src="https://user-images.githubusercontent.com/104331549/177235064-c486cd68-3e73-4970-b1dd-0b2eb6890c1f.png">

> 보통 웹어플리케이션은 클라이언트에 의해 요청이 들어오면,
> 서비스계층에서 데이터 엑세스 계층을 거쳐, 데이터베이스에 접근한다.
 - 데이터 엑세스 계층
   - JPA -> Hibernaate ORM(구현체) -> JDBC API 순
 
<br></br>
<br></br>
## JPA에서 P(Persisstence)의 의미 
 - Persisstence의  뜻은 영속성, 지속성이다.
   - 즉, 무언가를 금방 사라지지 않고 오래 지속되게 한다라는 것이 Persistence의 목적이다.

> 그렇다면 무엇을 오래 지속되게 하는 것일까?
 
### 영속성 컨텍스트(Persistence Context)
 - ORM은 `객체`와 `데이터베이스 테이블의 매핑`을 통해 `엔티티 클래스 객체` 안에 포함된 정보를 `테이블`에 저장하는 기술이다.
 - JPA에서는 테이블과 매핑되는 `엔티티 객체 정보`를 **영속성 컨텍스트에 보관** 해서 애플리케이션 내에서 오래 지속 되도록 한다.
   - 보관된 엔티티 정보는 데이터베이스 테이블에 데이터를 저장, 수정, 조회, 삭제 하는데 사용된다. 

### 영속성 컨텍스트 내부 구조

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177237128-0fb6ad76-7318-473c-9bd0-c5ced7cb891c.png" width="60%"></p>
 
 - `1차 캐시 영역`과 `쓰기 지연 SQL 저장소 영역`이 있다.

#### 1차 캐시 영역 
 - JPA API 중에서 엔티티 정보를 영속성 컨텍스트에 저장하는 API를 사용하면 영속성 컨텍스트의 1차 캐시에 엔티티 정보가 저장된다.

#### SQL 저장소 영역
<br></br>
<br></br>


# JPA API 영속성 컨텍스트 실습

## 사전준비 
### build.gradle 설정
```groovy
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // 추가! 
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
 - 위와같이 추가하면 기본적으로 Spring Data JPA 기술을 포함해서 JPA API를 사용할 수 있다.
> Spring Data JPA가 아닌 JPA API만 사용하고 싶다면 JPA 관련 의존 라이브러리를 별도로 추가해야된다.

### JPA 설정(application.yml)
```yaml
spring:
  h2:
    console:
      enabled: true
      path: /h2     
  datasource:
    url: jdbc:h2:mem:test
  jpa:
    hibernate:
      ddl-auto: create  # (1) 스키마 자동 생성
    show-sql: true      # (2) SQL 쿼리 출력
```
 - (1)과 같이 설정을 추가해 주면 애플리케이션 실행시, 이 엔티티와 매핑되는 테이블을 데이터베이스에 자동으로 생성해준다.
   - JDBC에서는 schema.sql파일을 이용해 테이블 생성을 위한 스키마를 직접 지정해 주었지만, 
   - JPA를 사용하고 (1)처럼 설정을 추가해주면 자동으로 DB 테이블을 생성해준다. 
 - `jpa.hibernate.ddl-auto: create` 관련하여 추가 사항은 [여기 Click](#ddlAuto)
<br></br>

## 영속성 컨텍스트에 엔티티 저장 
> 먼저 테이블과 매핑할 객체 엔티티 부터 만들어 주자


- Member.class
```java
import lombok.Getter; 
import javax.persistence.*;  // @GeneratedValue ,   @Id , @Entity

@Getter
@Setter
@NoArgsConstructor
@Entity  // 엔티티 클래스 필수
public class Member {
    @Id  // 엔티티 클래스 필수
    @GeneratedValue  //  식별자를 생성해주는 전략을 지정할 때 사용
    private Long memberId;

    private String email;

    public Member(String email) {
        this.email = email;
    }
}
```

> 그다음 영속성 컨텍스트를 관리하는 쿨래스(Configuration)을 만들어주자    
> @Configuration은 @Bean들을 싱글톤으로 관리해준다.   


- JpaBasicConfig.java 
  
```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;

@Configuration
public class JpaBasicConfig {
    private EntityManager em;
    private EntityTransaction tx;

    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) { // 의존관계 주입
        this.em = emFactory.createEntityManager();  // 의존관계 주입된 구현체로 초기화

        return args -> {
            Member member = new Member("hgd@gmail.com");
        
            // 영속성 컨텍스트에 해당 객체의 정보 저장
            em.persist(member);

	        // 영속성 컨텍스트에 저장된 정보 확인하기
            Member resultMember = em.find(Member.class, 1L);
            System.out.println("Id: " + resultMember.getMemberId() + ", email: " + 
                    resultMember.getEmail());
        };
    }
}
```
- 위 작업은, 영속성 컨텍스트안에 엔티티를 저장해보고, 다시 영속성 컨텍스트안에서 꺼내여 확인해보는 단계로 실행을 시켜보면
- 아래와 같이 로그창이 뜬다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177252230-ce6fca4e-43fd-45b2-a955-d5fb1c3e12d1.png" width="60%"></p>

- 하지만, **실제 데이터베이스에는 값이 들어가지 않는다.**

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177251906-76213d1c-ca40-4b3a-8746-215d6308304c.png"></p>

- 오히려 아래 그림처럼 영속성 컨텍스트 캐시안에서, SQL저장소로 꺼내어져 출력된 것 뿐이다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177251978-539cb6aa-c1bc-407e-875a-7e1a5c07b913.png" width="60%"></p>

- 위 그림과 로그를 확인해보면, 영속성 컨텍스트에 member 객체를 저장하지만 실제 데이터베이스에  insert 쿼리를 했다는 로그도 보이지 않는다.

<br></br>

### 영속성 컨텍스트와 테이블에 엔티티 저장
> 이번엔 실제 테이블에도 저장해보자!

- JpaBasicConfig.java
  - 이번엔 전에 사용하지 않았던 `EntityTransaction tx`를 이용한다.
```java
@Configuration
public class JpaBasicConfig {
    private EntityManager em;
    private EntityTransaction tx;


    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        return args -> {
            tx.begin(); // EntityTransaction 을 시작한다.
            Member member = new Member("hgd@gmail.com");

            em.persist(member);
            tx.commit(); //커밋을 날려준다.

            Member resultMember1 = em.find(Member.class, 1L);
            System.out.println("Id = " + resultMember1.getMemberId() + ", email: " + resultMember1.getEmail());

            Member resultMember2 = em.find(Member.class, 2L);
            System.out.println(resultMember1 == null); // primaryKey 1번이 있는지 확인  
            System.out.println(resultMember2 == null); // primaryKey 2번이 있는지 확인
        };
    }
}

``` 
 - 결과 log창
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177255327-e54431e7-52f5-4a49-ac1c-f57e7301d166.png" width="60%"></p>

 - `tx.commit()`를 추가한 후,이번엔 확실히 `Insert`쿼리가 실행되었음을 로그로 확인할 수 있다.
 - 게다가, 조회하는 방법은 
   - `resultMember1`의 경우 영속성 컨텍스트에 존재하기에 그것을 끌어다 사용하지만
   - `resultMember2`의 경우 영속성 컨텍스트에 없기에, DB 테이블에 한번 더 조회해본다. (하지만, DB 테이블에도 없음)


<br></br>

### 쓰기 지연을 통한 영속성 컨텍스트와 테이블에 엔티티 일괄 저장
> 위에는 영속성 컨텍스트 내부의 1차 캐시에 저장되는 방식이었다.  
> 이번에는 쓰기 지연 기능을 확인해 보자!  

- JpaBasicConfig.java
```java
// 위는 동일
   @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        System.out.println("# Active Profile: basic");
        return args -> {
           tx.begin(); // EntityTransaction 을 시작한다.
           Member member1 = new Member("hgd1@gmail.com");  
           Member member2 = new Member("hgd2@gmail.com");  
   
           em.persist(member1); // 1번 영속성 컨텍스트에 저장
           em.persist(member2); // 2번 영속성 컨텍스트에 저장
           tx.commit(); //커밋시, 1번 insert, 2번 insert 적용된다. 
        };
     }
```
 - `tx.commit()`을 하기 전까지는 `em.persist()`를 통해 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리가 실행이 되지 않는다. 
 - 즉, 아래와 같이 데이블에 데이터가 저장되려면 `tx.commit()`이 수행되야 한다.
 - 그리고 `tx.commit()`이 실행된 이후에는 쓰기 지연 SQL 저장소 실행된 쿼리는 제거된다.

 - 결과
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177256688-a4c64a3c-00bd-43da-8fd9-9d24b563282e.png" width="60%"></p>

<br></br>

### 영속성 컨텍스트와 테이블에 엔티티 업데이트
> 이미 테이블에 저장되어 있는 데이터를 JPA를 이용해서 어떻게 업데이트 할까?

- 방법은 간단하다. 조회해서 가져오고, 수정해서 다시 커밋해주면된다. 
- 예제 코드를 보자 

- JpaBasicConfig.java
```java
// 위는 동일
@Bean
public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        return args -> {
           tx.begin();
           em.persist(new Member("hgd1@gmail.com"));    
           tx.commit();   
   
           tx.begin();  // 엔티티 트랜잭션 시작
           Member member1 = em.find(Member.class, 1L);  //  조회
           member1.setEmail("hgd1@yahoo.co.kr");  // 수정
           tx.commit();   // 커밋
        };
}
```
 - 여기서 중요한 것은 조회하는 작업이, 테이블에서 조회하는 것이 아닌, 먼저 영속성 컨텍스트에서 조회를 수행한다.
   -  만약 조회되지 않는다면, DB테이블을 조회할 것이다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177258267-d62b77dd-e46c-4b69-950b-b9e21fe4050a.png" width="60%"></p>
 
 - `EntityTransaction`을 다시 시작해줘야하며, 조회한 값을 수정한 뒤 `tx.commit()`을 수행하면, 
 - 위 로그처럼 `UPDATE`쿼리가 실행된다. 

> 여기서 똑같은  `tx.commit()`문을 실행하지만 `UPDATE`쿼리가 실행되는 이유는 무엇일까?
 - 답은 영속성 컨텍스트에 있다.
 - 이미 전에 있던 떠놓은 스냅샷이랑 비교한 후, 변경된 값이 있으면 `쓰기지연SQL 저장소`에서 `UPDATE 쿼리`로 등록한다. 
 - 그래서 `tx.commit()`으로 SQL문이 실행될 때,  `UPDATE 쿼리문`으로 실행된다.

### 영속성 컨텍스트와 테이블에 엔티티 삭제 
> 마지막으로 삭제를 알아보자.
> 영속성컨텍스트와 DB 테이블 삭제는 어떻게 할까?

- JpaBasicConfig.java
```java
// 위는 동일
@Bean
public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        return args -> {
        tx.begin();
        em.persist(new Member("hgd1@gmail.com"));
        tx.commit();

        tx.begin();  // 엔티티 트랜잭션 시작
        Member member1 = em.find(Member.class, 1L);  //  조회
        em.remove(member1);  // 삭제 요청
        tx.commit();   // 커밋
        };
        }

```

 - 영속성 컨텍스트 1차 캐시에 해당 엔티티를 먼저 생성하고, 커밋이 있을때 DB테이블에 생성해준 것 처럼 
 - 삭제 또한, 영속성 컨텍스트 1차 캐시에 먼저 해당 엔티티를 제거 요청을 하고 `tx.commit()`이 수행되면,
   - 1차 캐시에 있는 엔티티 제거
   - 쓰기지연 SQL저장소에 등록된 `DELETE쿼리` 실행
 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177260381-238847ac-76d0-4267-a065-1701e676a92e.png" width="60%"></p>


> ### EntityManager의 flush() API 
>   
> 우리가 예제 코드에서 사용한 tx.commit() 메서드가 호출되면   
> JPA 내부적으로 em.flush() 메서드가 호출되어 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.






# 참고학습 

## ddl-auto <a name="ddlAuto"></a>
- [스프링 공식 문서링크](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto.data-initialization.using-hibernate)
- 결론부터 말하자면, `spring.jpa.hibernate.ddl-auto: create` 옵션은 로컬환경(인메모리 DB)에서만 사용해야 된다.
- 왜냐하면, `Create` 옵션은 해당하는 기존 테이블이 있으면 `DROP`하고 새로운 걸 만들기 때문이다.
   - 만약 운영DB라면, 기존DB가 사라지는 격이다. :scream:
### ddl-auto 옵션 종류
- `create`: 기존테이블 삭제 후 다시 생성 (DROP + CREATE)
- `create-drop`: create와 같으나 종료시점에 테이블 DROP
- `update`: 변경분만 반영(이것 역시 `운영DB`에서는 사용하면 안됨)
- `validate` : 엔티티와 테이블이 정상 매핑되었는지만 확인
- `none`: 사용하지 않음(사실상 없는 값이지만 관례상 none이라고 한다.)

### 정리 
 - 운영 장비에서는 절대 `create`, `create-drop`, `update` 사용하면 안된다.
 - 개발 초기 단계는 create 또는 update
 - 테스트 서버는 update 또는 validate
 - 스테이징과 운영 서버는 validate 또는 none
 - DDL(Data Defination Language)_데이터 정의어  = 데이터베이스의 생성,변경,삭제를 자동으로 생성해주는 것.


## Jakarta Persistence<a name="jakartaPersistence"></a>
> JPA(Java Persistence API)의 약자이지만 현재는 `Jakarta Persistence`라고도 불리는 이유를 알아보자

- [스택오버플로우 참고링크](https://stackoverflow.com/questions/60021815/why-has-javax-persistence-api-been-replaced-by-jakarta-persistence-api-in-spring)
 
- 이유를 보면, 
  - Java EE(기업용)가 Oracle에 의해 오픈 소스로 제공되고
  - Eclipse Foundation(이클립스 재단)에 권한이 부여된 후,
  - `Oracle`이 Java 브랜드에 대한 권한을 갖고 있기 때문에 Java에서 이름을 변경해야 하는 법적 의무
  - 자카르타라는 이름은 커뮤니티에서 선택

<p align="center"><img src ="https://user-images.githubusercontent.com/104331549/177279726-12292514-6132-4e97-ad4c-780da5b0f0dc.png"></p>


- [위키백과 참고링크](https://en.wikipedia.org/wiki/Jakarta_Persistence)

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177277791-066380c4-99e6-4a80-94fc-20169eb3da32.png" width="60%"></p>

- 2022년 7월 기준
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177276997-b7c6cfcc-5563-4489-9d79-06f2a3777115.png" width="60%"></p>
- JPA는 이미 Jakarta 패키지에 포함되어 있다.


## JPA 생명주기 
- 별도로 다룰 예정 [참고링크만 참조](https://thorben-janssen.com/entity-lifecycle-model/)