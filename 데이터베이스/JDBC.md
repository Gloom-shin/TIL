# 데이터 Access 기술 
> Spring의 데이터 접근 기술을 이야기하면 JDBC와 JPA가 주로 나오는데, 여기서 JDBC의 뜻은  
> Java Database Connectivity 으로 자바에서 데이터베이스에 접속할 수 있도록하는 자바 API이다.    
> 즉, Spring만의 기술이 아니라는 것이다.

## 목차 
 - [JBDC란?](#definition) 
   - [동작원리](#operation)
   - [JDBC 드라이버](#driver)
 - [JDBC API 사용 흐름](#APIflow)
 - Connection Pool()

<br></br><br></br>

# JDBC란? <a name ="definition"></a>
 - 원래 데이터베이스에 데이터를 저장하고, 수정하고와 같이 사용하려면, 이에 맞는 코드레벨을 사용해야되지만, Java 코드에서 사용할 수 있도록 해주는 해주는 API이다
 - Java에서 제공하는 표준 기술이다. 
    - 초창기(JDK1.1)버전부터 제공되어 왔다. 
 - JDBC API를 이용하여 다양한 벤더(Oracle, MS SQL, MySQL)등의 데이터베이스와 연동 할 수 있다. 

> 여기서, JDBC 기술이 오래되었다는 걸 알 수 있듯이, 현재는 실무에서 보다 좋은 기술로 발전시켰고, JDBC API는 잘 사용하지 않는다고 한다.   
> 하지만, 어떻게 동작하는지 흐름을 기반으로 발전시켰기 때문에, 흐름정도를 알고 가자  

<br></br>

## JDBC 동작원리<a name ="operation"></a>
 - 아래 그림과 같이 JDBC API를 사용하여 데이터베이스에 엑세스하는 단순한 구조이다.
 - JDBC 드라이버를 먼저 로딩한 후 데이터베이스와 연결하게 된다.

<p align="center"><img src ="https://user-images.githubusercontent.com/104331549/176591350-9953841c-461d-4983-aad9-c9fce15b7431.png" width="70%"></p>

### JDBC 드라이버(JDBC Driver)<a name="driver"></a>
 - Java 프로그램의 요청을 DBMS(데이터베이스매니저시스템)가 이해할 수 있는 프로토콜로 변환해주는 사이드 어댑터이다.
   - 데이터베이스(서버) - 요청하는쪽인 서버(클라이언트) 에서, JDBC 드라이버는 클라이언트 머신에 설치되어 있어야 한다. 
 - JDBC 드라이버는 데이터베이스와의 통신을 담당하는 인터페이스
 - Oracle이나 MS SQL, MySQL 같은 다양한 벤더에 맞는 드라이버를 구현해 제공된다. 
 - JDBC 드라이버의 구현체를 이용하여 데이터베이스에 엑세스 할 수 있다.

## JDBC API 사용흐름<a name="APIflow"></a>
 - JDBC로 데이터베이스에 접근하는 방법은 아래와 같다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/176592093-cf002df8-7008-4212-ad11-27e1d02da430.png" width=" 40%"></p>
 
### 1. JDBC 드라이버 로딩
 - 사용하고자하는 JDBC 드라이버를 로딩한다.
 - JDBC 드라이버는 DriverManager라는 클래스를 통해서 로딩된다.
### 2. Connection 객체 생성
 - JDBC 드라이버가 정상적으로 로딩되면 DriverManager를 통해 데이터베이스와 연결되는 세션(Session)인 Connection 객체를 생성
 - 여기서 세션은 사용자의 로그인 정보나 접속시간등 서버가 알아야할 정보를 저장하는 기능을 말한다. [참조 링크](https://devuna.tistory.com/23)
### 3. Statement 객체 생성
 - 작성된 SQL 쿼리문을 실행하기 위한 객체로, 정적인 SQL 쿼리 문자열을 입력으로 가진다.
### 4. Query 실행
- 생성된 Statement 객체를 이용해서 입력된 SQL쿼리를 실행한다.
### 5. ResultSet 객체로부터 데이터 조회
 - 실행된 SQL 쿼리문에 대한 결과 데이터 셋
### 6. ResultSet 객체 Close, Statement 객체 Close, Connection 객체 Close
 - 사용 이후는 역순으로 close해준다.



#  Connection Pool이란?<a name="connectionPool"></a>
 - 흔히 DBCP(DataBase Connection Pool)이라 불리며, conntion 객체를 관리하는 풀이다.
   - 웹 컨테이너(WAS)가 실행되면서 일정량의 Connection 객체를 미리 만들어서 pool에 저장했다가, 클라이언트 요청이 오면 Connection 객체를 빌려주고 해당 객체의 임무가 완료되면 다시 Connection 객체를 반납 받는다.
   - 관리하는 방법중에 `Hikari CP`의 경우, 이전에 사용했던 Conntion이 존재하는지 확인하고, 먼저 반환하는 특징이 있다.
   - 현재 Spring Boot 2.0 버전부터 Hikari CP`를 기본 DBCP로 사용하고 있다. 
 <p align="center"><img src="https://user-images.githubusercontent.com/104331549/176595223-e482d54d-def9-449f-b4c3-9d465aa588e4.png" width="70%"></p>

> 여기서 Connection을 간단히 짚고가자

### Connection 
  - JDBC API를 사용해서 `데이터베이스와의 연결`을 위한 Connection 객체를 생성한다. 
  - 그래서 내부적으로, 연결정보를 담는 URL(데이터베이스위치)과 같은 정보와 해당 벤더에 맞는 드라이버를 가지고 있다. 
  - 보통 `DriverManager.getConnetion()`메소드를 통해 **연결상태를 Connection 객체**로 표현하여 반환한다. 
     - 가장 부하가 많이 걸리는 과정이다. 
  - Statement객체를 생성하는 기능을 제공한다. 
  - SQL문을 데이터베이스에 전송하거나, 이러한 SQL문을 커밋하거나 롤백하는데 사용
  - 보통 Connection 하나 당 트랜잭션 하나를 관리한다.
    - 그리고 `Close`가 이루어지면 ConntionPool에 반납한다. 

## 커넥션 풀의 장점
 - 미리 만들어 연결하여 메모리상에 등록해 놓기 때문에, 불필요한 작업이 사라진다. => 클라이언트가 빠르게 DB에 접속이 가능함.
 - DB Connection 수를 제한할 수 있어서 과도한 접속으로 인한 서버 자원 고갈 방지가 가능
 - DB 접속 모듈을 공통화하여 DB 서버의 환경이 바뀔 경우 쉬운 유지 보수가 가능
 - 연결이 끝난 Connection을 재사용함으로써 새로 객체를 만드는 비용감소

## 유의사항 
 > 동시 접속자가 많을 경우
 - 커넥션은 한정되어 있기 때문에, 클라이언트가 사용할 커넥션이 반납될 때가지 기다려야 한다.(마치 비디어대여점)
 - 그렇다고, 미리 많은 커넥션을 생성해놓으면 메모리를 차지하여 자원이 낭비되고, 성능을 떨어뜨린다.

|표|커넥션풀이 클 경우| 커넥션 풀이 작을 경우|
|--|--|--|
|메모리| 많이 사용함| 적게 사용함|
|대기시간| 대기시간이 짧음 | 대기시간이 김|

 - 더 자세한 사항은 해당 [커넥션풀 링크 참조](https://steady-coding.tistory.com/564)
