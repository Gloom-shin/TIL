# Spring Rest Docs 실습
> 실습전에 기본적인 생성흐름을 파악하고 있어야한다.

- 한번더 정리하자면, API 문서 Spring Rest Docs 생성흐름은
  - Test코드 작성 -> RestDocs 코드 추가 -> 테스트코드 실행 
  - -> Build파일의 snippets 확인 -> 생성된 `.adoc`파일 사용하여, `index.adocs` 작성 
  - -> gradle의 `bootJar` 혹은 `build`를 실행하여, `index.html` 생성

## 테스트 코드 작성
```java
@WebMvcTest(MemberController.class)   
@MockBean(JpaMetamodelMappingContext.class)   
@AutoConfigureRestDocs
public class MemberControllerRestDocsTest {
  //...
}

```

- 슬라이스테스트에는 `@webMvcTest`를 적용하기 좋다. 
  - `@SpringBootTest`의 경우 `@AutoConfigureMockMvc`와 함께 사용되어 프로젝트에서 사용하는 전체 Bean을 ApplicationContext에 등록하여 테스트에 사용한다.
    - 통합테스트에 어울린다. 
  - `@webMvcTest`의 경우 필요한 Bean만 ApplicationContext에 등록하기때문에, 실행 속도가 상대적으로 빠르다.
    - 필요한 빈을 등록해줘야하는데, Controller를 테스트 하는 경우라면, 서비스계층으로 접근 하였을때 사용되는 JPA Bean들을 만들어줘야한다.
    - `@MockBean(JpaMetamodelMappingContext.class)` 으로 처리 가능
- JPA Bean을 만들어줄 `@MockBean(JpaMetamodelMappingContext.class)` 적용해준다.
  - 실제로는 JPA에서 사용하는 Bean들을 Mock 객체에 주입해주는 형태이다.
  - 또한, 메인 클래스에 [@EnableJpaAuditing](#EnableJpaAuditing)가 추가되어 있어 JPA에 필요한 정보를 자동으로 주입해주지만, `@WebMvcTest`의 경우에는, 실행시켜주지 않기때문에 `JpaMetamodelMappingContext.class`를 꼭 `@MockBean`으로 주입시켜줘야한다.
  




# 참고 링크
## @EnableJpaAuditing<a name= "EnableJpaAuditing"></a>
> @EnableJpaAuditing 를 하나하나 분석해보면, `Enable` `Jpa` `Auditing`으로 나눌 수 있다.   
> 여기서 `Enable`은 가능하게 만들어 주는 뜻이고, `Auditing`은 감시,감사 라는 뜻인데, 
> 보통 `Jpa Auditing` 으로 표현한다. 

### JPA Auditing이란?
- 먼저 JPA는 java에서 대표적인 ORM기술로, (도메인 -> 관계형 데이터베이스 테이블) 로 매핑할 때, 공통적으로 도메인들이 가지고 있는 필드나 칼럼들이 존재한다.
  - 대표적으로 생성일자, 수정일자, 식별자 같은 필드를 말한다.
- 데이터베이스에는 누가(식별자), 언제(시간)사용하였는지 기록을 잘 남겨놓아야 한다.
- 여기서 `JPA Auditing`은 Spring Data JPA에서 시간 혹은 식별자에 대해서 자동으로 값을 넣어주는 기능이다.
  - 원래라면, 도메인을 영속성 컨텍스트에 저장하거나 조회를 수행한 후에 update를 하는 경우 매번 시간 데이터를 입력하여 주어야 하는데, 
  - `JPA Auditing`을 이용하면 자동으로 시간과 식별자를 매핑하여 데이터베이스의 테이블에 넣어주게 된다.

### Auditing 사용법 
- 매핑은 자동으로 가능해졌지만, 모든 엔티티 클래스에, 생성일자, 수정일자 필드를 만드는 것은 비효율적이다. 
- 중복이 된다는 말이다. 그래서 실제로 중복될 데이터는 직접 추상클래스를 만들어줘서 해결할 수가 있다.
- 
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class Auditable {
    @CreatedDate
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    @Column(name = "LAST_MODIFIED_AT")
    private LocalDateTime modifiedAt;
}
```
- `@EntityListeners(javax.persistence)`
  - Entity를 DB에 적용하기 이전, 이후에 커스텀 콜백을 요청할 수 있는 어노테이션
  - `Class AuditingEntityListener (org.springframework.data.jpa)`
    -  Entity 영속성 및 업데이트에 대한 Auditing 정보를 캡처하는 JPA Entity Listener
- `@MappedSuperclass (javax.persistence)`
  - Entity에서 Table에 대한 공통 매핑 정보가 필요할 때 부모 클래스에 정의하고 상속받아 해당 필드를 사용하여 중복을 제거

> 실제 엔티티 객체에서는 추상클래스를 상속받아 사용하면, 자동으로 생성일자와 수정일자가 DB에 등록 및 매핑 된다.

### Auditing 식별자 처리
> 보통 식별자라고하면, `@ID`값으로 보통 하나씩 증가하는 시퀀스`SEQUENCE` 기법이나, Auto_increment와 같은 방식으로 처리하였다.  
> 하지만, 메모리적 효율과 보안적인 부분으로 식별자를 `UUID`로 많이 사용한다고 한다.

 - UUID 값을 만들어줘서, 식별자에 넣어주면 된다.
#### UUID 생성 
```java
import java.util.UUID;

public class test {
  private String uuid;
  public void init() {
    uuid = UUID.randomUUID().toString(); // uuid 생성
  }
}
```

#### UUID식별자를 싱글톤 빈으로 관리 
- `@Configuration`으로 관리 되는 곳에 생성
- `@Bean`으로 주입할 수 있는 메소드내에 생성

```java
@Configuration
public class AppConfig {
  @Bean
  public AuditorAware<String> auditorProvider() {
    //return ()-> Optional.of(UUID.randomUUID().toString());

    return new AuditorAware<String>() {
      @Override
      public Optional<String> getCrruentAuditor() {
        return Optional.of(UUID.randomUUID().toString());
      }
    };
  }
}
```





### 참고링크
 - [Auditing기능 - Tstory](https://web-km.tistory.com/42)
 - [UUID관련하여_최하단](https://github.com/backend-developer-study/spring-core/blob/e4c3e553c66d3ce2dfb64eb459b7b3524a2fe0ae/section09/09_%EB%B9%88%20%EC%8A%A4%EC%BD%94%ED%94%84_%EC%8B%A0%EC%B0%BD%ED%98%B8.md) 
