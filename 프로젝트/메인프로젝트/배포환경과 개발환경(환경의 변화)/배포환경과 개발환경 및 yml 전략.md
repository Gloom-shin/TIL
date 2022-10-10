# 개발환경과 배포환경
> 애플리케이션을 만들다보면, 개발환경과 실제 배포환경이 다른 경우는 비일비재하다.   
> 대표적으로, 로컬 개발환경에서는 테스트를 위해 인메모리 DB를 사용할 수 있지만, 서비스 배포환경에서는 인메모리 DB를 잘 사용안한다.  
> (DB가 다운되면 데이터가 다 날아가기때문에)   
> 이렇듯, 개발환경과 배포환경의 분할이 필요할때, 어떻게 해야되는지 알아보자

<br></br>
<br></br>


## Profile 
 - 이 문제를 해결하기위해 Spring에서는 프로파일(Profile) 이라는 기능을 제공한다.
 - 먼저 프로파일(Profile)기능을 사용하기전에, 파일이 세팅되어 있어야한다. 
 - 저는 아래와 같이, 개발용.yml과 테스트용.yml를 만들었다. 이름은 직관적으로 본인이 정하면된다.
   - 단, 여기서 중요한 것은 Spring에서 프로파일을 생성할때, `application-dev.yml` 처럼 ‘-(대시)’를 기준으로 프로파일명을 yml 파일 이름안에 포함하는 것이란걸 기억하자
   
<img src="https://user-images.githubusercontent.com/104331549/193213731-877c5107-f9d7-4464-b827-c2b4ea11fba9.png">

<br></br>

### Profiles 적용
 - 내가 적용하고픈 프로파일을 아래와같이 그룹별도 active 시킬수 있다.
 - `active : "dev"` 이면, `application.yml` 기본이 실행되면서, 프로파일이, `application-dev.yml`를 찾아 실행시켜준다. 
   - `active : "test"` 이면, 프로파일이, `application-test.yml`를 찾아 실행시켜준다.

```yaml
spring:
      profiles:
         active: "dev"
```

- 또한, 설정에서 명령어로 지정해준다면, `--spring.profiles.active=dev` 으로 지정해줄 수 가 있다.  
  <img src ="https://user-images.githubusercontent.com/104331549/193220892-30fd63fc-8bbb-4de9-a773-d3eef8c461e3.png">

  

<br></br>

### default Profile
- 위에서 느꼈다싶이, 대시이후에 아무것도 표현하지 않은 것이 `default`임을 알 수 있다.
  - 즉, `application-.yml` 이 `default`라는 것이다. 실제로 파일명을 `application-.yml`으로 바꿔도 이 yml파일이 기본값으로 실행됨을 알 수 있다. 
    - 여기서 또 `-`(대시)는 구분자를 위한 것 뿐이다.
- 그러므로 `active`된 프로파일이 없다면, 기본값으로 실행된다.
    - 기본 프로파일의 이름은, 아무것도 없는 상태이며, "none"으로 칭한다.
   - 설정에 `--spring.profiles.default`가 기본값이다. 아무런 입력을 하지 않아도 적용된다.
```yaml
spring:
  profiles:
    default: "none"
```

<br></br>


### profiles 이점
- 이렇게 되면, `default` yml파일에는 활성화 시킬 파일명령어만 입력해주면된다.(active에 여러개의 값이 들어 갈 수도 있다.)
    - 결국 `yml`파일도 역할에 따라 분할이 가능하다는 것이다.
```yaml
# 예시이다. 
spring:
      profiles:
         active: "local", "common", "test"
```
> 물론 `yml`분할도 너무 세분화가 많이 하게 되는 경우(Fine-Grained),  
> 다소 번거로울 수 있는데(한줄이 점점 길어지는 현상이 일어남. 특히나, yml의 장점을 못살린다.)   
> 이것을 해결해주는 `Group기능`도 제공해준다.

<br></br>
<br></br>

## Profile Groups
 - 여러개의 profile을 하나로 묶는 명령어로 `group:`을 추가해주고, `group`이 되는 `profile group명`과, 속하게되는 `profile 구성원`을 입력해주면된다.
```yaml
# Group 예시
spring:
  profiles:
    group:
      production: # Group 명 
      - "proddb" # profile 구성1
      - "prodmq" # profile 구성2
    # - 이하 생략     
```

 - 이렇게 하나하나, 세로로 작성할 수 있고, 또한 실행 시킬때는, `production` 하나만 실행시키면 된다.
 - command line 명령어로는 `--spring.profiles.active=production`을 입력해주면된다.


<br></br>

## Profile Include
 - 현재 실행하고 있는 `profiles` 즉, yml 파일이 있는데, 다른 `yml`의 설정에서 일부만 필요한 경우에는  `include` 명령어를 통해 실행합니다.
 
```yaml
# Include 예시
spring:
  profiles:
    include:
      - "common"
      - "local"
```
 - 이 경우 default `yaml`이 실행되며, default yaml 설정이 제일 우선시 되며, 그 이후 default에 추가되지 않은 설정은 `common`, `local`순으로 우선시 되어 적용이 된다. 
 - 이로써, `yaml`을 대체하는 것 대신, 추가하여 사용할 수도 있다.

> 언제 사용하게 될지 모르겠지만, 그래도 숙지는 해주자.

<br></br>
<br></br>


# yml profiles 추가 자료 
 - 스프링 부트 2.4 출시노트부터, 애플케이션 구성파일(application.yml 혹은 application.properties)의 작동방식이 변경되었다.
   - (2022.10기준 Spring boot 2.7.4 버전을 주로 사용하고 있다.)

## 변경된 구성 파일 처리 방식

### YAML 에서는 구분자(---)
 - 스프링 부트 2.4 이전 버전에서는 구분자(---)를 이용해서 문서를 구분짓는다. 파일 하나에서 구분자(—-)를 이용해서 문서를 여러 문서를 작성할 수 있었다.
   - 이를 이를 활용하여 spring.profiles: {profile}: 로 활성화되는 프로파일에 맞춰 애플리케이션 속성 정의문서를 작성할 수 있었다.

### 예시
```yaml
# application-oauth2.yml
spring:
  profiles:
    include: naver, kakao, google
 
---
spring:
  profiles: naver
  security:
    oauth2:
      client:
        registration:
          naver:
		        # client id, client-secret, redirect-uri, scope, client-name
        provider:
          naver:
            # authorization_uri, token_uri, user-info-uri, user_name_attribute
     
---
spring:
  profiles: kakao
  security:
    oauth2:
      client:
        registration:
          naver:
		        # client id, client-secret, redirect-uri, scope, client-name
        provider:
          naver:
            # authorization_uri, token_uri, user-info-uri, user_name_attribute
---
spring:
  profiles: google
  security:
    oauth2:
      client:
        registration:
          naver:
		        # client id, client-secret, redirect-uri, scope, client-name
        provider:
          naver:
            # authorization_uri, token_uri, user-info-uri, user_name_attribute
```


 - 하지만, 2.4버전 이후부터는 YAML 파일에 spring.profiles 을 deprecated 되어 있다.
   - <img src="https://user-images.githubusercontent.com/104331549/194004084-5281f8fe-1882-49c1-8155-f7945fe65ddf.png">
   - 이말은, 구분자 기능을 그대로 사용할 수 있지만, spring.profiles 기능을 사용할 수 없기에, 구분지어 문서를 작성하는 방식이 더이상 유효하지 못하다는 것이다.
 - 스프링 부트 2.4에서 외부 설정 파일 관련된 변화가 있다. 좀 더 직관적으로 알아챌 수 있도록 프로퍼티명이 변경됐다
   - `spring.profiles` 가 `deprecated` 되고 `spring.config.activate.on-profile` 로 변경됨

## spring.config.activate
- 어찌보면, 다들 스프링 부트 2.4이 되고나서 부터 `spring.config.activate.on-profile`를 사용하라고 권장하지만, 공식문서에는 다음과 같이 적혀 있다.
<p align="center"> <img src="https://user-images.githubusercontent.com/104331549/194008841-516e9bb3-8581-425e-8ffb-dd174e447c87.png" width="70%"></p>

- 애플리케이션을 설정할때, 가끔 특정한 조건에서는 지정된 yaml 집합만 활성화하는 것이 필요한 경우가 있을 텐데, 그때 사용하는 것이다. `spring.config.activate.*` 이다.
  - 즉, 해당 yaml(설정파일)만 사용하고 싶을때 사용한다는 것이다.
- 게다가, `spring.config.activate.*` 설정에는 2가지 종류가 있다.
  - `on-profile` 
  - `on-cloud-platform`
- 둘 다 해당 파일만 사용하고 싶을 때 사용하는 것으로 큰 차이는 크게 없으며, `on-cloud-platform`의 경우 cloud-platform 설정파일을 넣어주기만 하면 된다.

### 주의점 
 - 위에서 언급했던, `spring.profiles.active` 랑 
 - 이번에 언급한 `spring.config.activate.on-profile` 은 같이 사용할 수 없다.
```yaml
# 잘못된 문서 예시
spring:
  config:
    activate:
      on-profile: "prod"
  profiles:
    active: "metrics"
```

<br></br>

- 또한, `spring.config.activate.on-profile` 에 여러개의 yml파일이 올 수 있는 지 확인해보자
- 두개든, 한개든 애초에 yaml에 아래와 같이 설정을 넣어봐도, 실행 시켜 보면 인식하지 못한다.
```yaml
spring:
  config:
    activate:
      on-profile: s3 # or s3, h2
```

<p align="center"> <img src="https://user-images.githubusercontent.com/104331549/194020496-939b12dd-fcf6-4288-92a9-a5b4e509993d.png" width="70%"></p>


### 올바른 Profile yaml 설정
 - 만약 아래와 같이, yml파일이 5개가 존재하는데, 이중에 s3랑 h2만 실행하고 싶다면
    - application.yml
    - application-h2.yml(실행하고 싶은 yml)
    - application-mysql.yml
    - application-s3.yml(실행하고 싶은 yml)
    - application-test.yml
    - 
<img src="https://user-images.githubusercontent.com/104331549/194043127-8abeb588-d14a-4e6b-84d4-f6f8c100bad1.png">

```yaml
spring:
profiles:
  active: h2, s3
```
  <p align="center"> <img src="https://user-images.githubusercontent.com/104331549/194030437-34f153ac-1df6-4975-be75-bb2155667dc4.png" width="70%"></p>

- 이상없이 여러 yml이 실행된다.
> 그럼 대체 `spring.config.activate`의 `on-profile`은 언제 사용하는 것일까?

## on-profile


  <p align="center"> <img src="" width="70%"></p>


## 참고 자료 
 - 참고용 : [공식spring.profile 페이지](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.profiles)
 - `@Configuration(proxyBeanMethods = false)`, `@Profile("프로파일명")`을 통해 부분적으로 적용도 가능하게 보인다.
- [spring boot 2.4 profile 적용 안되는 문제](https://theheydaze.tistory.com/512)
- [spring-boot 2.4 부터 변경된 구성파일 처리방식 살펴보기](http://honeymon.io/tech/2021/01/16/spring-boot-config-data-migration.html)
