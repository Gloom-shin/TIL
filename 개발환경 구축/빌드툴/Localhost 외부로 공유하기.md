# 내 로컬호스트를 외부로 공유하는 법 
> 내 로컬 PC에서 ip를 외부로 공유하는 방법은 여러가지가 있는데, 무료로 쓸수 있으며,주로 쓰인다는   
> 로컬터널에 대해 알아보자

## 로컬터널(node 패키지)

- node.js 랑 NPM 이 설치 되어 있어야댐
  - 노드 js 설치하면 npm가 같이 설치된다.

### 로컬터널 설치
- `npm install -g localtunnel`
- `-g`는 글로벌 옵션이다.

<img src="https://user-images.githubusercontent.com/104331549/187347993-e90367f1-0ace-4ca6-a67d-8563a4a61f75.png">


### 포트열기 
 - `lt --port 4000`
   - `https://slick-laws-care-222-117-186-4.loca.lt` 과 같은 localtunnel에서 제공해주는 url로 나옴.
### 도메인 변경
- `--subdomain flag`
- ex)
  - `lt --port 4000 --subdoamin gloom`
  - `https://gloom.loca.lt/` 와 같은 변경된 URL이 나옴
 

### 로컬터널 접속 
 - 나타나는 url를 가지고 접속하면, 아래와 같은 페이지가 나온다. 

 <img src="https://user-images.githubusercontent.com/104331549/187348519-b6dfea87-6aa0-42a1-b12b-37eb66e86912.png">


## CORS에러 
- 백엔드와 프론트엔드가 나눠져있거나, 이와 비슷하게 서버가 나눠져 있는 상황이라면, CORS에러를 발견할 수가 있다.
- 이 부분도 크게 2가지 방법으로 해결할 수 있는데 
  - 하나는 프록시 서버를 만들어서 처리하는 방법이고
  - 다른 하나는 백엔드 서버에 접근권한을 주는 방법이다. 

### 백엔드에서 권한 부여
> Spring java로 개발할 경우 아래와 같이 간단하게 해결가능하다. 
 - 보통 Controller를 통해 프론트와 소통함으로 Controller에 접근 권한을 부여해주면 된다. 
 - `@CrossOrigin` 을 사용하면 된다.
   - 속성같으로는 `@CrossOrigin(origins="*", allowedHeaders = "*")` 이런 식으로 줄수 있으며, 보통 테스트환경에서 하기에, `*` 모두에게 권한을 부여해준다.

