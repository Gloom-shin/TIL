# 하이버네이트 
 - 자바 언어를 위한 객체 관계 매핑 프레임워크(ORM)으로,
   - 객체 지향 도메인 모델 -> 관계형 데이터베이스(RDB)로 매핑할때 쓰인다.
 - JPA의 구현체중 하나로, SQL을 직접사용하지 않고, 메서드 호출만으로 쿼리를 수행한다. 
    - 내부적으로 SQL문이 돈다.

> 그런데, 굳이 하이버네이트를 공부하게된 이유가 무엇인가?

<br></br>

## 문제점 발견
> 애초에 하이버네이트를 쓸일은 데이터베이스와 접근할 때 쓰이게 되는데,
> 평소 Spring을 사용하면서 `H2`와 연동하다가, `MySQL`로 연동을 바꾸는 과정에서 문제가 발생하였다.

### 연동 DB 변경
 - Spring이 실행되면서 연동될 DB가 `Mysql`임을 설정해줘야하기에 
   - datasource값에 mysql관련된 주소, 접근 할 수 있는 아이디 및 비밀번호
   - 그리고 지원하는 jdbc 드라이버를 설정값에 입력해줬다.
 - application.yml
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test
    username: shin
    password: ****
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: create  # 스키마 자동 생성
    show-sql: true      # SQL 쿼리 출력
    properties:
      hibernate:
        format_sql: true  # SQL pretty print
```


### 오류발생
 - `Access to DialectResolutionInfo cannot be null ` `when 'hibernate.dialect' not set`
 - 파파고의 힘을 빌려 해석해 보자면
   - 접근할 Dialect해결정보는 null일 수 없습니다. 
   - 'hibernate.dialect'이 세팅되지 않았다.

> `dialect`가 먼지 검색해보니까 , 사투리,방언으로 나온다.  
> 맥락이 이상해지길래, 결국 하이버네이트의 dialect가 뭔지 찾아보게 되었다.  


<br></br>
<br></br>

## Hibernate. dialect
 - 하이버네이트가 데이터베이스와 통신을 하기 위해 사용하는 언어를 Dialect라고 한다. 
 - 모든 데이터베이스에는 각자의 고유한 SQL언어가 있는데, 관계형 데이터베이스끼리 형태나 문법이 어느정도 비슷하긴 하지만, 완전히 똑같지는 않다.
   - 예를 들어 Oracle 쿼리 구문과 MySQL 쿼리구문은 다르다. 
 - 하지만, 하이버네이트는 한 데이터베이스관리시스템(DBMS)에 국한되지않고, 다양한 DBMS에 사용 가능하다. 
   - 내부적으로 각자 다른 페이징 방법을 처리해주고 있는 것이다.
   - 덕분에 특정 벤더(DBMS)에 종속적이지 않고, 얼마든지 대체가능하다.
 - JPA에서는 아래와 같이 Dialect라는 추상화된 언어 클래스를 제공하고 각 벤더에 맞는 구현체를 제공하고 있다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/179675617-b16281f3-de1e-4efb-a6c5-294017d3584d.png" width="80%"></p>

<br></br>

## 해결방안
 - 위 오류는 결국 MySql SQL를 처리해줄수 있는 `dialect`의 값이 제대로 설정되지 않아서 나타나는 오류임을 알수 있다.
 - 그래서 JPA에 값을 지정해주면 해결이 된다.

 - 해결한 application.yml
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test
    username: shin
    password: ****
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    database: mysql   # 추가 해준 부분
    database-platform: org.hibernate.dialect.MySQL5InnoDBDialect # 추가 해준 부분

    hibernate:
      ddl-auto: create  
    show-sql: true      
    properties:
      hibernate:
        format_sql: true  
```
 - 만약 Oracle로 바꾸는 상황이었다면 `OracleDiect`를, H2로 바꾸는 상황이었다면 `H2Dialect`의 platform를 찾아 넣어주면된다.
 - 보다, 많은 상황은 아래 링크를 참조하자.
## 참고자료
[Hibernate 교육 해외사이트](https://www.educba.com/hibernate-dialect/)