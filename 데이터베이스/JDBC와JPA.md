# Data Access 기술 유형
> Spring에서는 데이터베이스에 접근하는 다양한 기술들을 사용할 수 있다.   
> 대표적인 기술로는 `mybatis`, `Spring JDBC`, `Spring Data JDBC`, `JPA`, `Spring Data JPA` 등이 있다.  

- 이러한 기술들의 뿌리로 올라가, 만들어지는 목적을 보고 기반을 보면 크게 2가지로 나눌 수 있다. 
- `SQL 중심 기술`과 `Object 중심 기술`

<br></br>
<br></br>

# SQL 중심 기술
 - SQL 중심 기술을 기반으로한 대표적인 데이터 접근 기술은 `mybatis`와 `Spring JDBC`가 있다.
 - 말 그대로, SQL 쿼리문 중심 기술로, 애플리케이션 내부에서 직접적으로 작성하는 것이 중심이 되는 기술이다.
 - 예를 들어 `mybatis`의 경우 
    - `SQL Mapper`라는 설정 파일에 SQL쿼리문을 직접적으로 작성해줘야 했다.
    - 작성된 SQL 쿼리문을 기반으로 데이터베이스의 특정 테이블에서 데이터를 조회한 후, 다시 Java객체로 변환해주는 기술이다.
 - 또다른 예시로 `Spring JDBC`경우
   -  `JdbcTemplate`이라는 템플릿 클래스를 사용하여 SQL쿼리문을 작성해줘야했다.
 > 즉, **코드에 SQL 쿼리문이 포함되어야 한다**.

# 객체(Object) 중심 기술
 - 객체(Object) 중심 기술은 데이터를 SQL 쿼리문 위주로 생각하는 것이 아니라 모든 데이터를 객체(Object) 관점으로 바라보는 기술이다.
 - 쉽게 말해, SQL쿼리문을 직접적으로 작성하는 것이 아닌, 객체를 이용해 내부적으로 SQL쿼리문으로 자동변환 한 후 데이터베이스의 테이블에 접근하는 방식이다.
 - 이러한 객체(Object)중심의 데이터 접근 기술을 `ORM(Object-Relational Mapping)`이라고 한다.
> 대표적인 Java ORM 기술로는 JPA(Java Persistence API)가 있다.  
> 즉, 직접적인 SQL 쿼리문을 다루는 일이 적어지게된 것이다.
