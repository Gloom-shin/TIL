# API계층 테스트

- 웹 애플리케이션을 MVC패턴으로 만들게되면, 크게 3개의 계층으로 나눌 수 있는데,
- API 계층, 서비스계층, 데이터 엑세스 계층이다.
- 이중, HTTP요청을 받고 서비스계층으로 분류 작업을 해주는 Controller작업이 있는데,
- 이부분을 테스트 하기위해서는 HTTP 요청값과 서비스계층의 반환이 필요하다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/184801704-a26aaca5-6ae4-47e8-a0b1-287441b1eb58.png" width= 70%></p>

<br></br>

# Controller Test

- 위에서 보다싶이, Controller를 테스트하기위해선 크게 2가지가 필요하다.
    - 가짜 HTTP 요청
    - 가짜 Service 반환(`Mock`으로 사용하면됨)
- 이 중을 HTTP 요청을 만들어 내기위해서, 사용하는 타입이 있는데 `MockMvc` 라고 한다.

## MockMvc

- 서블릿 컨테이너의 구동 없이, 시뮬레이션된 MVC 환경에 모의 HTTP 서블릿 요청을 전송하는 기능을 제공하는 유틸리티 클래스이다.
    - 즉, 가짜 HTTP 요청을 만들수 있다는 것이다.
- 가짜 HTTP 요청에 필요한 구성
    - HTTP Method
    - HTTP URL
    - HTTP Header
    - HTTP Body(필요하다면)

## HTTP 서블릿 요청을 위한 MockMvc 메소드

### perform()

- MockMvc가 제공하는 메서드로, 브라우저에서 서버에 **URL 요청**을 하듯 컨트롤러를 실행시킬 수 있다.
- RequestBuilder 객체를 인자로 받고, 이는 MockMvcRequestBuilders의 정적 메소드를 이용해서 생성한다.
- 반환값이 인터페이스 `ResultActions` 이다.

#### MockMvcRequestBuilders 객체 인자

- import 하게되는 패키지 `org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;`
- `MockMvcRequestBuilders`의 메소드들은 GET, POST, PUT, DELETE 요청 방식과 매핑되는 **get(), post(), put(), patch(), delete()**,
  options(),head() 등의 메소드를 제공한다.
- 이 메소드들은 `MockHttpServletRequestBuilder`객체를 리턴한다.
  <p align="center"><img src="https://user-images.githubusercontent.com/104331549/185034557-ad127e26-10f0-4a18-867f-1b044ec5346b.png" width= 70%></p>

    - 그리고 이 `MockHttpServletRequestBuilder`객체는 아래와 같이 HTTP 요청 관련 정보를 설정할 수 있다.
  ```java
    // 모두 가져오기에는 양이 많아 요약해봤습니다.
    public class MockHttpServletRequestBuilder implements ConfigurableSmartRequestBuilder<MockHttpServletRequestBuilder>, Mergeable {
         private final String method;// http 요청 메소드
          private final URI url; // URL
          private String contextPath = ""; // 컨텍스트경로
       private String servletPath = ""; //서블릿경로
          @Nullable
          private String contentType; // 요청헤더 인코딩 타입
          @Nullable
       private byte[] content; // 바디
          private final MultiValueMap<String, Object> headers = new LinkedMultiValueMap<>(); // 헤더

       private final MultiValueMap<String, String> parameters = new LinkedMultiValueMap<>(); // 파라미터

       private final MultiValueMap<String, String> queryParams = new LinkedMultiValueMap<>(); //쿼리 파라미터

       private final List<Cookie> cookies = new ArrayList<>(); // 쿠키
       private final List<Locale> locales = new ArrayList<>(); // 로컬
   // 이하 생략

   }
  ```
    - 이와 같이 HTTP 요청 관련 정보를 파라미터, 헤더, 쿠키, 로컬 등 다양하게 정의하여 만들 수 있다.
    - 값이 없다면, 기본값을 넣어준다.

#### contentType()

- 위 `MockHttpServletRequestBuilder`객체의 HTTP메시지(요청,응답 모두 포함한) 헤더 `content type`을 설정한다.

#### accept()

- 위 `MockHttpServletRequestBuilder`객체의 HTTP 요청 메시지(요청만) 헤더 `content type`을 설정한다.

#### content()

- 위 `MockHttpServletRequestBuilder`객체의 바디 `content`을 설정한다.
- @Nullable로 널값을 넣을 수도 있다.

### 예시

```java
mockMvc.perform(get("/members") // URL과 HTTP Method 구현
        .contentType(MediaType.APPLICATION_JSON)  // 메시지 헤더
        .accept(MediaType.APPLICATION_JSON) // 요청 메시지 헤더
        );
```

> 추가적으로, RestDocs를 사용한다고 하면, MockMvcRequestBuilders를 사용하지 않고  
> `import org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.get;`   
> Restdocs안에 있는 패키지를 사용해야한다.
> 반환값은 MockHttpServletRequestBuilder 객체로 동일 하다.

<br></br>

### ResultActions

- 실행된 요청 결과에 대한 기댓값 검증이나, 가공할 수 있다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185040205-61109a9d-69f9-4954-b5e6-ec0569fdcdba.png" width= 70%></p>

#### andDo()

- 인자로 들어온 작업을 수행한다.
- 일반적으로 RestDocs를 만들때, andDo()로 실행한다.

#### andExpect()

- `ResultMatcher`타입을 인자로 가지며, 기댓값을 넣어 검증하는 작업을 수행한다.
- 테스트의 결과를 확인할 때 쓰인다.

#### andReturn()

- 실행된 요청의 테스트결과를 객체로 반환한다.
- 즉 테스트한 결과를 객체로 받을 때 사용한다.

## MockMvcResultMatcher
 - 검증할 때 쓰이는 다양한 메소드를 제공한다.
### 응답 상태 코드 검증

|메소드|설명|
|:---:|:---:|
|isOk()|응답 상태 코드가 정상적인 처리(200)인지 확인|
|isNotFount()|응답 상태 코드가 404 Not Found인지 확인|
|isMethodNotAllowed()|응답 상태 코드가 메소드 불일치(405)인지 확인|
|isInternalServerError()|응답 상태 코드가 예외발생(500)인지 확인|
|is(int status)|몇 번 응답 상태 코드가 설정되었는지 확인 <br/>ex) is(200), is(404)|

> 추후 뷰템플릿을 사용하게되면 뷰템플릿 테스트에 대해서도 알아보자
### 뷰/리다이렉트 검증
 - view()
### 모델 정보 검증
 - attributeExists()
 - attribute()


- [MockMvcRequestBuilders공식문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/request/MockMvcRequestBuilders.html)
- [RestDocumentationRequestBuilders공식문서](https://docs.spring.io/spring-restdocs/docs/current/api/org/springframework/restdocs/mockmvc/RestDocumentationRequestBuilders.html)
- [ResultActions 공식문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/ResultActions.html)