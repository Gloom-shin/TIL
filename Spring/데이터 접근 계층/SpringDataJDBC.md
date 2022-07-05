# Spring Data JDBC
> 이 글을 보기전에, 데이터베이스의 JDBC.md 와  JDBC와JPA.md를 읽고오길 바란다.
> 또한, 스프링의 대표적인 데이터베이스 접근 기술    
> `Spring Data JDBC`: 비교적 최근에 릴리즈된 기술   
> `JPA` : 가장 많이 사용하는 기술     
>  `Spring Data JPA` : JPA를 보다 편리하게 사용하는 기술  
>  세가지를 앞으로 숙지해야한다.  
> 그중 먼저 `Spring Data JDBC`를 살펴보자

<br></br>
<br></br>

# Spring Data JDBC란?
- 이름에 JDBC를 가지고있지만,`Spring Data JDBC`는 객체 중심의 기술이다. 
- Spring Data JDBC 기술은 2018년에 1.0 버전이 처음 릴리즈 되었기 때문에 기술의 역사가 아직 짧은 편
- 애플리케이션의 규모가 상대적으로 크지 않고, 복잡하지 않을 경우에는 Spring Data JDBC가 뛰어난 생산성을 보여줄거라 기대한다.

<br></br>

# Spring Data JDBC를 사용해보기  

## 사전 준비
<br></br>
### 1.의존라이브러리 추가
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
    - 여기서 [인메모리(In-memory) DB란](#InMemoryDB) 

<br></br>

### 2.`application.yml` 파일에 H2 Browser 활성화 설정 추가
 - `src/main/resources 디렉토리` -> `application.properties`혹은 `application.yml` 파일
 - 이 파일은 Spring에서 사용하는 다양한 설정 정보들을 입력할 수 있다. 
 - Spring이 H2 DB에 접속하고 관리할 수 있도록 설정 정보를 아래와 같이 입력해준다.
```yaml
spring:
  h2:
    console:
      enabled: true
```
<br></br>

### 3. H2 DB 정상 동작 유무 확인
- H2 데이터베이스 연동이 잘 되었는지 체크 
- 보통 3가지를 확인하면 된다. 
  - 콘솔창 확인 
  - 웹 브라우저 console 확인
  - 웹 브라우저 DB 확인

#### 콘솔창 확인
 - 그냥 실행시켜보고 콘솔창을 확인하면 된다.

<img src="https://user-images.githubusercontent.com/104331549/176604491-a50b913b-cddc-4a09-a7d9-de18930ebb36.png">
 
 - 실행했을때 오류없이 콘솔창에 위와 같이 `H2 console available`이 나오면 된다.
 - 그리고 `at '/h2'.`을 기억하자.
#### 웹브라우저 console 확인
 - URL 입력 창에 `localhost:8080`과 그뒤에, 위에서 봤던 `'/h2'`를 붙여 `http://localhost:8080/h2/` 입력해주면 된다. 
 - 아래와 같은 콘솔창을 확인하면 된다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/176605250-4faab96f-3187-4d52-8016-e933e38d33aa.png" width="50%"></p>

####  DB 확인
 - 마지막으론 스프링 실행시 나오는 콘솔창에  `Database available at 'jdbc:h2:mem:test'` 에서 애초에 이 뜻이, Database을 available 할수 있는 url을 알려주는 log이다.
 - `jdbc:h2:mem:test`을 아래와 같이 JDBC URL에 입력해주고 connetion를 클릭해주면 DB관리창이 뜬다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/176607038-895be392-0f13-4204-b10c-c36ab8c12d27.png" width="50%"></p>

- DB 창
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/176607021-3936569e-9a57-4f2b-a764-fb80cf8b5056.png" width="50%"></p>

<br></br>
### 4. H2 DB 디폴트 설정 추가
 - H2 DB는 애플리케이션을 재시작 할때 마다 애플리케이션 로그에 출력되는 `JDBC URL`이 매번 랜덤하게 바뀌기 때문에, 매번 새로 입력해야한다. 
 - 이러한 문제는 `application.yml` 파일에 H2에 대한 추가 설정을 함으로써 해결할 수 있다.
```yaml
spring:
  h2:
    console:
      enabled: true
      path: /h2     # h2 console창 경로 지정
  datasource:
    url: jdbc:h2:mem:test     # JDBC URL 지정
```

<br></br>
<br></br>


## 샘플 코드 구현 


## 참조문구 

### 인메모리(In-memory) DB? <a name="InMemoryDB">
 - 원래 데이터베이스는 요청에의해 저장되고 삭제가 되기 때문에, 서버를 내렸다가 다시 가동시켜도 데이터베이스 안에 데이터가 그대로 유지된다.
 - 반면에, In-memory DB는 메모리 안에 데이터를 저장하는 데이터베이스이다. 
 - 메모리는 휘발성이기 때문에, 전원을 껐다키면 저장되어 있는 내용이 모두 지워지기때문에 테스트용도에 알맞는다. 
 - 만약 직접 MySQL과 같은 DB에 접근하여 테스트해볼 수도 있지만, 실무에서는 DB가 함부로 변하면 안되기 때문에, In-MemoryDB를 사용하는 것이 좋다.
 - 또한, 테스트내용을 직접 지우지않아도되는 이점이 있다.
