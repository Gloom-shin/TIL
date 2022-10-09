# H2와 MySQL
 > 메인프로젝트의 DB는 관계형데이터베이스로 진행하기로 해서,  
 > 로컬 개발환경에서는 H2를, 배포 환경에서는 MySQL를 RDBMS로 정해서 하기로 했다.  
 > 그런데, 분명 개발환경에서는 잘 돌아가던 프로젝트가, 배포환경에서는 에러가 나는 것이다.

### 문제점 
 - `Table [db명. 테이블명] doesn't exist` 에러 발생 
 
### 해결책 
 - 결론부터 말하자면, 별거 아닌문제이다.
 - H2는 대소문자를 딱히 구분하지않아도 테이블 조회가 가능했지만, MySQL의 경우 대소문자를 구분해주기 때문에, 대문자가 있으면 조회할 수 없어 없는 테이블로 결과가 나온다.
   - Spring @Entity로 만들어주는 Table들은 Class명에 맞춰서 소문자로 들어 가는데, 
   - 이번 프로젝트에 @JPQL 에서 native 쿼리를 사용할때에는 대소문자를 사용했기에 일어났다. 
   

### 해결책을 찾은 후 고민
 - 하지만, 이러한 문제는 개발환경과 배포환경이 다른 DB를 사용하게되거나, 다양한 DB를 사용하게되면 또 나타날 것이다. 
 - 물론 애초에 가장 좋은 방법은 **소문자**로 통일하는 것도 방법이지만, 그래도 JPA를 보다 깊게 이해하고, 또 다른 문제가 발생했을때, 도움이 되지않을까 하여, 어떻게 매핑되는지 알아보고자 한다.

<br></br>
<br></br>

## JPA의 Entity 
> 각, RDBMS 관련하여 들어가기 전에 JPA에서 Entity를 만들고 실제 만들어지는 Table이 어떻게 생성되는지 부터 알아보자
> 테스트해볼 DB는 H2, MySQL 이며, 환경설정을 다시 만들기 귀찮아서, 기존 MainProject에다가 임의로 만들었다.

### 테스트해볼 Entity 코드
```java
package mainproject.nosleep.entitytest.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import java.io.Serializable;
import java.math.BigDecimal;
import java.security.Timestamp;
import java.sql.Blob;
import java.sql.Clob;
import java.sql.Time;
import java.sql.Date;
import java.util.Calendar;
import java.util.Currency;
import java.util.Locale;
import java.util.TimeZone;

@Entity
public class TestEntity {
    @Id
    private Long id;

    @Column private String testString;
    @Column private Character testCharacter;

    @Column private Integer testInteger;
    @Column private Long testLong;
    @Column private Short testShort;
    @Column private Float testFloat;
    @Column private Double testDouble;
    @Column private Byte testByte;
    @Column private Byte[] testBytes;
    @Column private Boolean testBoolean;

    @SuppressWarnings("rawtypes")
    @Column private Class testClass;

    // java.sql
    @Column private Timestamp testTimestamp;
    @Column private Date testDate;
    @Column private Time testTime;
    @Column private Clob testClob;
    @Column private Blob testBlob;
    // java.math
    @Column private BigDecimal testBigDecimal;

    // java.io
    @Column private Serializable testSerializable;



    // java.util
    @Column private Calendar testCalendar;
    @Column private Locale testLocale;
    @Column private TimeZone testTimeZone;
    @Column private Currency testCurrency;
}
```

### H2

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/194712475-4a30e6fb-9000-4ad5-9d69-0a1786e29f7d.png" ></p> 

 - 위 화면은 H2에서 해당 Entity를 조회하여 보는 화면이며, 모든 테이블 명과 칼럼이 **대문자**로 이루어져있다. 
 - 또한, Java에서 작성한 `CamelCase`는 대문자를 중심으로 `SnakeCase`로 전환되어 있다.
 - 하지만 H2는 대소문자를 구분하지않기에 아래와 같이, `SELECT * FROM TEST_entity`로 조회해도 조회가 된다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/194742910-09de652a-ee74-4e1f-a877-800d73e99794.png"></p> 


### MySQL

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/194743038-8cb66a7b-91f1-48db-9245-2582e36d6ef6.png" width="70%"></p> 

 - 위 화면은 MySQL에서 해당 Entity를 조회하여 보는 화면이며, 모든 테이블 명과 칼럼이 **소문자**로 이루어져있다. 
 - H2와 마찬가지로, Java에서 작성한 `CamelCase`는 대문자를 중심으로 `SnakeCase`로 전환되어 있다.
 - MySQL도 쿼리문으로는 대소문자를 구분하지않기에 아래와 같이, `SELECT * FROM TEST_entity`로 조회해도 조회가 된다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/194743632-d2f98b1f-0f8e-4566-a1d6-172af8726498.png" width="70%"></p> 


> 예상과는 다르게, 테스트해본 결과 H2, MySQL 둘다 대소문자를 구분하지 않았다.   
> 그럼 대체 왜, 나는 대소문자를 구분하지 않고 조회하지 못하는 `Table [db명. 테이블명] doesn't exist` 에러가 발생했을까?    

<br></br>
<br></br>


## 다른 이유 알아보기
 - 개발환경과 배포환경에서 다른 것은 RDBMS 뿐만이 아니다, CPU와 메모리 같은 내부 스펙도 다르고 CLI, GUI등 많은 것이 다르다. 
 - 이번엔, 운영체제가 다른 것에 초점을 맞췄다. 
 - 기본적으로 로컬(개발)환경에서는 `Windows`로 사용하고 있다. 하지만 배포환경에서는 `Ubuntu` 즉, 리눅스 환경에서 배포한 것이다.

### 알아낸 원인
 - 기본적으로 `Windows`는 대소문자 구분을 하지 않는 게 기본 설정이지만,
 - 리눅스는 테이블 `name`조차도 파일로 관리하기 때문에 대소문자를 구분하는 게 기본 설정입니다.


### Mysql 설정 확인하는 법
 - 해당 운영체제에서 `Mysql`에 접속한다. 
 - Variable이라는 곳에 설정들이 보관되어 있는데, 대소문자 구분관련된 설정을 보기위해선 
   - `show variables like 'lower%';` 로 조회하면 된다.

 - 리눅스의 경우 
  <p align="center"><img src="https://user-images.githubusercontent.com/104331549/194744325-db92b5eb-7540-4ff8-94de-172b09aef37d.png" width="50%"></p>

 - Windows의 경우  
  <p align="center"><img src="https://user-images.githubusercontent.com/104331549/194744330-b6fc2960-39c1-49bb-a33b-195b605ea26e.png" width="50%"></p>

 - 확실히, Windows의 MySQL과 리눅스의 MySQL은 기본 세팅부터 차이가 있음을 알 수 있었다. 
 - 이 값을 변경 시킬라고 하면 `Read`만 가능한 파일이기에, 변경이 되질 않는다. `Error Code: 1238. Variable [설정명] is aread only variable`

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/194744602-5c577709-8eb4-487b-b33c-1e86d15bf875.png" width="70%"></p> 

 - 그래서, 값을 변경하고 싶다면, 직접 파일을 열어 직접 변경해야 한다. (권한을 바꾸는 방법도 있긴하다.)
 - 리눅스의 `/etc/mysql/my.cnf` 파일을 들어가서 아래와 같이 입력해주면 된다.

```shell
# Mysql server
[mysqld]
lower_case_table_names=1
```
 - 그리고 mysql를 restart 해주면 설정이 변경된다고 한다. 


> 하지만 여기서 의문점은, MySQL이 Windows에서는 대소문자를 구분하지 않지만, 굳이 리눅스에서는 대소문자를 구분하는 이유는 무엇일까?


<br></br>
<br></br>


## 이유
- 결론 부터 말하자면, MySQL에서 주로 사용하는 `database`와 `table`이 `Directory`와 `File`명이기 때문이다.
- 운영체제 `Windows` 에서는 디렉토리와 파일에 접근할때 대소문자 구분을 하지 않으나, `Linux/Unix` 는 구분하므로 select 나 insert 시에 테이블의 대소문자를 구분해야되는 것이다. 
- 결국, 리눅스 환경에서 MySQL과 같은 RDBMS를 사용한다면, 운영체제에 맞춰 대소문자를 구분하게 설정하는 것이 좋다는 것이다. 
  - 오히려, 개발환경에서 배포환경을 맞춰 따라가는게 더 맞음으로 windows의 MySQL의 설정을 바꾸는 걸 추천한다.


## 정리 
 - RDBMS의 SQL쿼리문의 대소문자를 구분하는 것은 설정으로 변경할 수 있다. 
 - 디렉토리와 파일관리를 할 때 운영체제 Windows는 대소문자를 구분하지않고, 리눅스는 대소문자를 구분한다.
 - 그러므로, 각 운영체제에 RDBMS 설치시, 환경에 맞춰 설정이 적용되는 것이다.


