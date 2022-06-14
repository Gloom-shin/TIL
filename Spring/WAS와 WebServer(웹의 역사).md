# WAS와 WebServer❓
> 솔직히 나는 WebServer만 알았지, WAS가 뭔지도 몰랐다. 그래서 먼저 개념부터 정리해보고자 한다.

### Web Server
 - 클라아언트의 요청(request)을 받아 **정적**인 컨텐츠(html,css,js)로 응답(response)하는 서버를 말한다.
 - 예) Apache, Nginx, IIS, WebtoB등이 있다.

### WAS
 - `Web Application Server`의 줄임말이다.
 - 클라이언트의 요청(request)을 받아 DB조회나, 어떤 로직을 처리해야하는 **동적**인 컨텐츠를 응답(response)하는 서버
 - 예) Tomcat, WebLogic, WebSphere, Jeus, JBoss등이 있다.

> 이렇게 봐썬, 아직 동적/정적 말고는 구분도 안되고, 감도 안온다.   
> 좀 더 범위를 확장하고 깊이를 얇게하여, 넓은 시야에서 바라봐보자

# 웹페이지의 역사
## 1세대 웹 
> 정적인 웹 
 - 웹이 탄생하고 대중적으로 알려졌을 때, 초창기 웹은 매우 정적이었다.
 - 단순히 정보 제공이 주 목적이다보니, 기능도 없었고, UI도 필요가 없었다. 
 - 그래서 대부분의 웹페이지들은 HTML과 CSS로 이루어졌다. 
 - 그러니 당연히 데이터전송도 HTML를 통해 이루어 졌다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/173523570-afd5aff3-1de2-4f6c-a866-0eaba034017b.png" width= 70%></p>


## 2세대 웹
> 동적인 웹
 - 보다 원활한 소통을 위해 반응하는 웹페이지가 필요 해졌다.
 - 그래서 다이나믹한 요소들을 구현할 수 있는 JavaScript 언어가 출현하였다. 
 - HTML , CSS, JavaScript를 활용하여 UI를 만들었다.(그래서 JSP사용) 
 - 이때만 해도 아직 프론트엔드/백엔드를 나눌 필요없이 개발하였다. 

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/173524023-3c693b53-2df5-4ead-a8a8-837c84f1154d.png" width= 70%></p>


## 3세대 웹 
> SPA (Single Page Application) 방식 등장
 - 단일의 HTML페이지로 전체 웹서비스를 구현할 수 있게 되었다. 
    - 즉, 하나의 페이지에서 동적으로 필요한 페이지를 구현할 수 있게되었다. 
 - 단일 HTML페이지에 메인 JavaScript파일을 포함하게 되었다.
 - 업무가 분업되기 시작함.
    - 웹 페이지 렌더링에 필요한 코드는 최초의 통신에서 한번에 송수신 한다.
    - 그 이후 소통하는 것들은 실시간으로 데이터를 주고 받으면서 필요한 화면을 **동적으로 송수신** 한다.
    - 즉, 이때부터 동적인 서버가 필요로 해짐
    
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/173525081-dbd6380b-f507-406b-8670-2fc92bb537ac.png" width= 70%></p>


### 프론트엔드와 백엔드 
 - 동적인 UI의 중요도가 높아지고, 다양해지면서 페이지 구성하는 서버의 비중이 커짐.
 - 자연스럽게 웹 브라우저가 페이지 구성에 필요한 서버와 실시간으로 데이터를 주고받을 서버가 나눠지면서
 - 프론트엔드와 백엔드의 업무가 분리하게됨.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/173531162-ee8f5de9-98a3-4cb1-9b64-f33dfec9a7ea.png"></p>

<br></br>  
## 웹페이지 분류 
> 웹페이지는 크게 뼈대가 되는 정적인 페이지와 실시간 변동이 가능한 동적인 페이지로 나눌 수 있다. 
 - Static Pages
 - Dynamic Pages

### Static Pages
> 바뀌지 않는 페이지 
 - Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
 - 항상 동일한 페이지를 반환한다.
 - Ex) image, html, css, javascript 파일과 같이 컴퓨터에 저장되어 있는 파일들


### Dynamic Pages
> 인자에 따라 바뀌는 페이지
 - 인자의 내용에 맞게 동적인 contents를 반환함
 - 웹 서버에 의해 실행되는 프로그램을 통해 만들어진 결과물임 (Servlet : was 위에서 돌아가는 자바 프로그램)
 - 개발자는 Servlet에 doGet() 메소드를 구현함

## 웹서버와 WAS 
> 그럼 웹서버와 WAS에 대해 어느정도 감이 올 것이다. 

### Web Server(웹서버)
 - 웹 브라우저 클라이언트로부터 HTTP 요청을 받고, 컨텐츠(html, css, js)를 제공하는 컴퓨터 프로그램
 - 크게 2가지 기능중 선택해서 제공
     1. 정적(뼈대) 컨텐츠 제공
        - WAS를 거치지 않고 바로 리소스를 클라이언트에 제공한다. 
     2. 동적 컨텐츠 제공을 위한 요청 및 전달 
        - 클라이언트 요청을 WAS에 보내고, WAS에서 처리한 결과를 다시 클라이언트에게 전달한다.
       
### WAS 
> Web Application Server의 약자
 - HTTP를 통해 애플리케이션을 수행해주는 미들웨어
 - WAS는 `웹 컨테이너` 혹은 `서블릿 컨테이너`라고도 한다(컨테이너란 JSP, Servlet을 실행시킬 수 있는 소프트웨어)
 - WAS의 역할은 웹서버 + 웹컨테이너 역할을 함.(그래서 프론트엔드 서버가 없어도 처리가 가능하긴함.)
    - 주로 보안, 스레드 처리, 분산 트랜젝션 등 분산환경에서 많이 사용한다.

#### 주요 기능
 - 프로그램 실행 환경 및 DB 접속 기능 제공
 - 여러 트랜잭션 관리 기능
 - 업무 처리하는 비즈니스 로직 수행 


## 사용 예시
### 웹서버
```
클라이언트가 이미지 파일(정적 컨텐츠)를 보낼 때..

웹 문서(html 문서)가 클라이언트로 보내질 때 이미지 파일과 같은 정적 파일은 함께 보내지지 않음
먼저 html 문서를 받고, 이에 필요한 이미지 파일들을 다시 서버로 요청해서 받아오는 것

따라서 웹 서버를 통해서 정적인 파일을 애플리케이션 서버까지 가지 않고 앞단에 빠르게 보낼 수 있음!
```
> 웹서버가 정적인 컨텐츠를 처리해줌으로써, 실질적인 서버의 부담을 줄여주는 역할을 한다. 

### WAS
```
동적인 컨텐츠를 제공해야 할때..

웹 서버만으로는 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어놓고 서비스하기에는 자원이 절대적으로 부족함

따라서 WAS를 통해 요청이 들어올 때마다 DB와 비즈니스 로직을 통해 결과물을 만들어 제공!
```



추가적으로 또한, 여러 대의 WAS를 연결지어 사용이 가능하다.
웹 서버를 앞 단에 두고, WAS에 오류가 발생하면 사용자가 이용하지 못하게 막아둔 뒤 재시작하여 해결할 수 있음 (사용자는 오류를 느끼지 못하고 이용 가능)

<img><p align="center"><img src="https://user-images.githubusercontent.com/104331549/173533630-c4ab2056-155f-4aba-91c9-389757b11e9a.png"></p>




### 참조 링크
[위키백과 java Servlet](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%84%9C%EB%B8%94%EB%A6%BF)
[위키백과 웹컨테이너](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
[velog](https://velog.io/@doniminp/%EC%9B%B9-%EC%84%9C%EB%B9%84%EC%8A%A4%EC%9D%98-%EC%97%AD%EC%82%AC%EC%99%80-%EB%B0%9C%EC%A0%84)
[velog2](https://velog.io/@uvula6921/%EB%8F%99%EC%A0%81-%EC%9B%B9-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%9E%80)
[깃허브블로그](https://doozi316.github.io/web/2020/09/13/WEB26/)
[깃허브블로그2](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
