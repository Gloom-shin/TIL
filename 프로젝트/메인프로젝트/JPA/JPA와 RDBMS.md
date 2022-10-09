# JPA와 RDBMS
- 사실 Java 진영에서 개발을 하다보면, 결국 데이터베이스에 값을 저장해야한다.
- 보통 데이터베이스로 (Oracle, MySQL 등)RDBMS를 이용하여 많이 구현하는데, Java의 객체화된 데이터를 관계형 데이터베이스로 저장하는 작업이 필요해진다. 
  - 그래서 이것을 위해 객체화된 데이터를 관계형데이터베이스에 저장하는 `SQL문` 중심적으로 개발하게되었다.

## 문제점 발생
> 결론 부터 말하자면 초기 구축 및 유지보수가 많이 힘들어 진다.
- Entity마다 CRUD를 위한 쿼리작성필요(SELECT, INSERT, UPDATE, DELETE)
- 객체를 SQL로 변환해주고, SQL를 객체로 변환해주는 과정이 매번 필요하다.
- 테이블의 칼럼 구성이 변경되면 작성된 모든 쿼리의 수정이 필요하다(노가다의 유지보수)
- 물론 이와같은 문제점들은 RDBMS를 사용시 일어나는 문제점들이다.


<p align="center" ><img src="https://user-images.githubusercontent.com/104331549/194750208-e4f4ea34-7913-43ab-a99b-cf7c98590135.png" width="70%"></p> 

<br></br>


### 문제점의 원인 
- 여러가지 문제점의 원인이 있겠지만, 가장 큰 원인중 하나는 객체와 DB의 차이점이라고 보면 된다.

|   차이점    |       Object        |                   RDBMS                    |
|:--------:|:-------------------:|:------------------------------------------:|
|    상속    |     상속관계가 존재한다.     | 상속관계가 존재하지않는다. <br/>(대신, 슈퍼타입-서브타입관계가 있다.) |
|   연관관계   | 참조를 통해 연관관계를 구현한다.  |          외래키(FK)를 통해 연관관계를 구현한다.           |
|  데이터타입   | Java의 데이터 타입에 맞춘다.  |  MySQL, H2, Oracle 각각의 리터럴한 데이터 타입이 존재한다.  |
| 데이터 식별방법 | JAVA에서 비교연산자는 `==`  |               SQL에서 비교연산자는 `=`               |
 
- 즉, 데이터 식별방법이 다르다는 것은 문법이 다르다는 것이다.


- 그래서 이러한 문제를 해결하기 위해 `JPA` 가 등장했다.
  - 쉽게 말해, JPA란 Java진영에서 RDBMS 로 변환하기 쉽게 해주는 것이다.
  - 이것을 객체 관계매핑이라고도 하는데, 흔히 알고 있는 ORM(Object-Relational Mapping)이다.
- JPA 덕에, 어떤 RDBMS가 오게 되더라도, 설정만 해주면 매핑이 된다는 것이다. 
> 하지만, Java의 데이터 타입과 RDBMS 사이에는 연관관계 하는 법도 다르고 데이터타입을 표현하는 것도 다 다르다. 특히, 데이터 타입의 경우에는 RDBMS 종류에 따라 다 다른데,
> 내가 메인프로젝트때 사용했던, H2와 MySQL은 어떻게 다른지 알아보자.   

<br></br>
<br></br>

### 테스트해볼 Entity 코드
 - 기존 코드를 그대로 활용하였다.
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


### MySQL
 - MySQL에 매핑된 데이터타입은 다음과 같다.
 - 일단 컬럼이 알파벳순으로 정렬되어 있다.
<p align="left" ><img src="https://user-images.githubusercontent.com/104331549/194751303-cfd71425-5be5-4524-91ea-6fb946f74915.png" width="50%"></p> 
