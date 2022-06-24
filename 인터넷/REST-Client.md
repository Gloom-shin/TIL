# 클라이언트(Client)와 서버(Server)의 관계
- 클라이언트란, 사용자가 보는 웹 브라우저를 말한다.
- WAS와 WebServer에 대해 이야기했었던거 처럼, WebServer는 보통 Frontend기능을 담당한다.
  - 웹 브라우저는 웹서버에게 컨텐츠(서버 쪽의 리소스(Resource, 자원))를 요청하고, 컨텐츠를 받아서 브라우저로 보여준다.

<img src="https://user-images.githubusercontent.com/104331549/175553129-f4540fea-51da-4ded-9c27-4b0ed91e6ac5.png">

> 서버는 항상 클라이언트에게 리소스를 제공하는 역할만 하는 것이 아니라  
>  **서버도 다른 서버로부터 리소스를 제공 받아야하는 경우가 굉장히 많다.**

## Frontend 서버와 Backend서버 
<img src="https://user-images.githubusercontent.com/104331549/175555546-2be244ea-404b-4000-80f6-6b4c68a88519.png">
 - 웹 브라우저 입장에서는 Frontend의 리소스를 제공받기 때문에 명백하게 클라이언트이다.
 - 하지만, Frontend서버도 동적인 데이터가 필요하여, Backend서버에게 데이터를 요청하게된다면, 
    - 이때 만큼은 Frontend가 Backend의 리소스를 이용하는 클라이언트가 되는 것이다.

> 결국, 클라이언트와 서버의 관계는 상대적이라는 것

> 추가 Tip  
> 클라이언트 `앱`을 만들기 위한 `React`, `Angular` 같은 자바스크립트 진영에서는 `Backend 서버와 통신하기 위해` `Axios` 같은 라이브러리를 사용


## Backend서버와 Backend서버 

<img src="https://user-images.githubusercontent.com/104331549/175556397-8fada376-b7db-4b6c-9b1a-84ab49ae9174.png">
- Backend도 모든 작업을 하나의 서버에서 처리하지않고, 또 다른 Backend서버에 HTTP요청을 전송해서 작업을 나누어 처리하는 경우가 많다. 
- 결국 Backend서버가 클라이언트가 되고, Backend서버가 리소스를 제공해주는 서버도 되는 것이다.


# Rest Client란?
- `Rest Client`란 말 그대로 Rest API 서버에 HTTP 요청을 보낼 수 있는 클라이언트 툴 또는 라이브러리를 의미합니다.
- 대표적으로 `Postman`은 UI가 갖춰진 `Rest Client` 이다.

> 그럼 위 그림과 같이 UI가 없는 Backend A의 애플리케이션 내부에서 Backend B의 애플리케이션에 HTTP 요청을 보내려면 어떻게 할까?  
> UI가 없는 Rest Client 라이브러리를 사용하면 된다.

## RestTemplate
 - Java에서 사용할 수 있는 HTTP Client 라이브러리
   - java.net.HttpURLConnection
   - Apache HttpComponents
   - OkHttp 3
   - Netty 등이 있다.
 - `Spring`에서는 이 HTTP Client 라이브러리 중 하나를 이용해서 원격지에 있는 다른 Backend 서버에 HTTP 요청을 보낼 수 있는 `RestTemplate`이라는 `Rest Client API`를 제공한다.
 - `RestTemplate`을 이용하면 `Rest 엔드 포인트 지정`, `헤더 설정`, `파라미터 및 body 설정`을 한 줄의 코드로 손쉽게 전송할 수 있

### 1. RestTemplate 객체 생성
```java
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;


public class RestClientExample01 {
    public static void main(String[] args) {
        // (1) 객체 생성
        RestTemplate restTemplate = 
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());
    }
}
```
- 기본적으로 `RestTemplate`의 객체를 생성하기 위해서는 
  - RestTemplate의 생성자 파라미터로 `HTTP Client 라이브러리`의 구현 객체를 전달해야한다. 
- `HttpComponentsClientHttpRequestFactory` 클래스를 통해 `Apache HttpComponents`를 전달한다.

> `Apache HttpComponents`를 사용하기 위해서는 `builde.gradle`의 `dependencies` 항목에 아래와 같이 의존 라이브러리를 추가해야한다.

```java
dependencies{
        implementation'org.apache.httpcomponents:httpclient'
}
```

### 2. URI 생성
- `RestTemplate` 객체를 생성했다면 `HTTP Request`를 전송할 **Rest 엔드포인트**의 URI를 지정해 주어야 한다.
```java
import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import java.net.URI;
...
// (2) URI 생성
        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
//                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();
    }
```
 - `UriComponentsBuilder`클래스를 이용해서 `UriComponents`객체를 생성한 후에 이 `UriComponents`객체를 이용해서 **HTTP Request를 요청할 엔드포인트의 URI**를 생성한다.

#### `UriComponentsBuilder`클래스의 API 메서드
- `newInstance()`
  - UriComponentsBuilder 객체를 생성합니다.
- `scheme()`
  - URI의 scheme을 설정합니다.
- `host()`
  - 호스트 정보를 입력합니다.
- `port()`
  - 디폴트 값은 80이므로 80 포트를 사용하는 호스트라면 생략 가능합니다.
- `path()`
  - URI의 경로(path)를 입력합니다.
  - URI의 path에서 {continents}, {city} 의 두 개의 템플릿 변수를 사용하고 있습니다.
  - 두 개의 템플릿 변수는 `uriComponents.expand("Asia", "Seoul").toUri();` 에서 expand() 메서드 파라미터의 문자열로 채워집니다.
  - 즉, 빌드 타임에 {continents}는 ‘Asia’, {city}는 ‘Seoul’로 변환됩니다.
- `encode()`
  - URI에 사용된 템플릿 변수들을 인코딩 해줍니다.
  - 여기서 인코딩의 의미는 non-ASCII 문자와 URI에 적절하지 않은 문자를 Percent Encoding 한다는 의미입니다.
- `build()`
  - UriComponents 객체를 생성합니다.

#### `UriComponents`추상클래스의 API 메서드
 - `expand()`
   - 파라미터로 입력한 값을 URI 템플릿 변수의 값으로 대체한다.
 - `toUri()`
   - URI 객체를 생성한다.


### 3. 요청 전송
 - `getForObject()`를 이용한 문자열 응답 데이터 전달 받기
```java
        // (3) Request 전송
        String result = restTemplate.getForObject(uri, String.class);

        System.out.println(result);

```
#### `getForObject()`
 - HTTP Get 요청을 통해 서버의 리소스를 조회한다.
 - 파라미터 
   - `URI uri`
     - Request를 전송할 엔드포인트의 URI 객체를 지정해 줍니다.
   - `Class<T> responseType`
     - 응답으로 전달 받을 클래스의 타입을 지정해 줍니다.
     - 코드 3-19에서는 응답 데이터를 문자열로 받을 수 있도록 String.class로 지정했습니다.

### 4.결과값
<img src="https://user-images.githubusercontent.com/104331549/175561577-b467d4c7-4392-49d6-836e-10b7a91877f5.png">


### 5. 정리 
 - RestTemplate을 사용할 수 있는 기능 예
   - 결제 서비스
   - 카카오톡 등의 메시징 서비스
   - Google Map 등의 지도 서비스
   - 공공 데이터 포털, 카카오, 네이버 등에서 제공하는 Open API
   - 기타 원격지 API 서버와의 통신
