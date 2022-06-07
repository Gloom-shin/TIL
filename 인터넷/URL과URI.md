## URI(Uniform Resource Identifier)
- 특정 리소스를 식별하는 통합 자원 식별자를 의미하며, 문자열 시퀸스다.
- 일반적으로  URL의 기본요소인 scheme , hosts, url-path에 더해 query, bookmark를 포함한다. 

## URL(Uniform Resource Locator)
 - 네트워크 상에서 웹페이지, 이미지, 동영상 등의 파일이 어디 있는지 위치한 정보를 나타낸다. 
 - URI의 서브셋이다. 
 - 인터넷 URL에 `file://localhost/C:\Users/username\Desktop\`을 입력하면, PC의 폴더와 파일을 탐색할 수 있다.
 
 <img src="https://user-images.githubusercontent.com/104331549/172292758-6c357b01-13f4-470b-a8b9-c9ede1ed7792.png" width =40%>
 
## URI 와 URL 차이점
 
> URI는 식별하는 것이고, URL은 위치를 가르키는 것이다.   
> 즉, 모든 URL은 URI가 될수 있지만, 모든 URI는 URL이 될 수 없다. 

- 내가 이해한 URL과 URI는 
- URI의 경우 해당 파일이 있는 경로까지를 말하며, 
- URL의 경우 해당 파일의 확장자까지 언급하여, 파일을 가르키게끔 입력됨을 말한다.(즉, 자원의 위치)
- 실제론, 자원의 위치(해당 파일)까지 외부로 노출시킬 필요가 없으므로, URI에서 `query`문을 통해 필요한 정보만 조회하는 것이다. 

## URI의 구조

`scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]`

(ex) 
`file:///C:/Users/shin/Desktop/`
`https://www.google.com:443/search?q=JavaScript`
|part|설명|예시|
|--|--|--|
|scheme|사용할 프로토콜을 뜻하며, `통신프로토콜`이라함|`filel://`  `https://`|
|user, password(선택)|접근하기위한 사용자의 이름과 비밀번호||
|hosts|웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹서버, 도메인 또는 IP|`127.0.0.1`   `www.google.com`|
|port|웹 서버에 접속하기 위한 통로|`:80`  `:443`  `:3000`|
|url-path|웹 서버의 루트 디렉토리로부터 웹페이지, 이미지, 동영상 등의 파일이 위치까지의 경로|`/Users/shin/Desktop/`  `/search`|
|query|웹 서버에 전달하는 추가 질문|`q=JavaScript`|

<br></br>
### 참조 링크
https://blog.lael.be/post/61
