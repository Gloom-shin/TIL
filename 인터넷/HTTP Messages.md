# HTTP(HyperText Transfer Protocol)
- HTML을 비롯하여, 이미지, 동영상, XML문서, JSON 등 다양한 데이터를 문서를 전송하기 위한 Application Layer 프로토콜(일종의 규칙!)이다.
- 웹브라우저와 웹 서버의 소통을 위해 디자인 되었다.

## HTTP 동작
 - 본래 멀리 떨어져있는 사람끼리 지식을 공유하기 위해 만든 시스템으로, 서로 주고 받는것이 중요한 기능이였다.
 - 그리하여 지금까지도, 크게 2가지 동작을 하는데, 그것이 요청과 응답이다.

<img src= "https://user-images.githubusercontent.com/104331549/172310070-6a5f7068-a734-41b8-9ede-d97cfdfd73d0.png">

## HTTP 특징
- HTTP 메시지는 HTTP서버와 HTTP 클라이언트에 의해 해석됩니다.
- TCP/IP를 이용하는 응용 프로토콜입니다.
- HTTP는 연결 상태를 유지라지 않는 비연결성 프로토콜입니다..
    - (이러한 단점을 해결하기 위해 Cookie와 Session이 등장함)
- HTTP는 연결을 유지하지않는 프로토콜이기 때문에 요청/응답 방식으로 동작한다.

## HTTP messages 구조
- 요청과 응답은 유사한 구조를 가진다.

<img src= "https://user-images.githubusercontent.com/104331549/172312622-39bec130-f7cf-4d57-8abf-94b4fd23a7a3.png">

  1. start line : start line에는 요청이나 응답의 상태를 나타냅니다. 항상 첫 번째 줄에 위치합니다. 응답에서는 status line이라고 부릅니다.
  2. HTTP headers : 요청을 지정하거나, 메시지에 포함된 본문을 설명하는 헤더의 집합입니다.
  3. empty line : 헤더와 본문을 구분하는 빈 줄이 있습니다.
  4. body : 요청과 관련된 데이터나 응답과 관련된 데이터 또는 문서를 포함합니다. 요청과 응답의 유형에 따라 선택적으로 사용합니다.

- 이중 1.start Line 과 HTTP headers를 묶어 요청이나 응답의 `헤드(head)`라고 하고, payload는 `body`라고 한다.


### 공통 헤더 
- `General headers` : 메시지 전체에 적용되는 헤더로, body를 통해 전송되는 데이터와는 관련이 없는 헤더입니다.
- `Representation headers` : Entity headers로 불렀으며, body에 담긴 리소스의 정보(콘텐츠 길이, MIME 타입 등)를 포함하는 헤더입니다

<img src ="https://user-images.githubusercontent.com/104331549/172322570-7976f612-dd79-4ad6-b0c8-98465f2693dc.png">


## 요청(Requsets)
<img src ="https://user-images.githubusercontent.com/104331549/172319745-21abcb23-dcd5-4a13-a44d-60e3eac0b2e0.png">
 
### startLine

 - 서버가 특정 동작을 취하게끔 만들기 위해 클라이언트에서 서버로 보내는 메시지이다. 
 - 크게 3가지 요소로 이루어져 있다. (ex. POST / HTTP/ 1.1)
    1. **첫번째**는 서버가 수행하는 동작을 나타낸다.            
        - HTTP 메서드로 `GET`, `POST`, `PUT`, `DELETE` 등이나 방식으로 `HEAD`, `OPTIONS` 올 수 있다. 
    2. **두번째**는 요청 타켓의 URL, 또는 프로토콜, 포트, 도메인의 절대 경로가 올 수 있다.
        - origin : `POST / HTTP / 1.1` `GET /background.png HTTP/1.0` `HEAD /test.html?query=alibaba HTTP/1.1` `OPTIONS /anypage.html HTTP/1.0`   
        - absolute 형식: 완전한 URL 형식(대부분 GET과 함께 사용됨) `GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1`
        - authority 형식 : 도메인 이름과 포트가 있는 URL이 사용된다. HTTP 터널을 구축하는 경우에만 CONNECT와 함께 사용할 수 있다 `CONNECT developer.mozilla.org:80 HTTP/1.1`
        - asterisk 형식 : OPTIONS와 함께 별표('*') 하나로 간단하게 서버 전체를 나타냅니다. `OPTIONS * HTTP/1.1`
    3. 마지막 **세번째**는 `HTTP 버전`이 들어간다. (2022년기준 최신버전은 HTTP/1.3이다.)
 - 하지만, 요즘 HTTP개발모드를 보면, UI가 바뀌어서 위의 3가지 요소를 선택적으로 볼 수 있다.
 - HTTP 버전은 2.0이 많이 쓰인다. 

<img src ="https://user-images.githubusercontent.com/104331549/172322008-96167781-733a-4c84-95de-af90bebe526a.png">


#### Header
 - Header 파일은 네트워크 파트에서 아무 파일이나 클릭하면, 정보를 볼수 있는데, 첫페이지가 Header이다. 
    - 일반적인 Header의 정보가 나오고, 그 뒤론 각각 요청과 응답의 정보를 볼 수 있다. 
- `Request headers` : fetch를 통해 가져올 리소스나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더를 의미합니다. User-Agent, Accept-Type, Accept-Language과 같은 헤더는 요청을 보다 구체화합니다.


### Body
- Single-resource bodies(단일-리소스 본문) : 헤더 두 개(Content-Type과 Content-Length)로 정의된 단일 파일로 구성됩니다.
- Multiple-resource bodies(다중-리소스 본문) : 여러 파트로 구성된 본문에서는 각 파트마다 다른 정보를 지닙니다. 보통 HTML form과 관련이 있습니다

## 응답(Responses)
### Status line
 - 응답 첫줄 또한 크게 3가지 요소로 이루어져 있다.
   - `요청` 처럼 현재 프로토콜의 버전을 출력하는 건 동일하다. 
   - 상태 코드 - 요청의 결과를 나타낸다.(ex. 200, 302, 404 등)
   - 상태 텍스트 - 상태 코드에 대한 설명(ex. Not Found)
  
### Headers
 - `Response headers` : 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더로, Vary, Accept-Ranges와 같이 상태 줄에 넣기에는 공간이 부족했던 추가 정보를 제공합니다


### Body
 - 응답 코드가 201, 204와 같은 상태 코드를 가지는 응답에는 본문이 필요하지 않다.
 - 응답의 body는 크게 두종류로 나눌 수 있다
    1. Single-resource bodies(단일-리소스 본문) 
       - 길이가 알려진 단일-리소스 본문은 두 개의 헤더(Content-Type, Content-Length)로 정의
       - 길이를 모르는 단일 파일로 구성된 단일-리소스 본문은 Transfer-Encoding이 chunked 로 설정되어 있으며, 파일은 chunk로 나뉘어 인코딩
    2. Multiple-resource bodies(다중-리소스 본문)   
       - 서로 다른 정보를 담고 있는 body입니다.
  

### 참고자료 
https://developer.mozilla.org/ko/docs/Web/HTTP/Messages
