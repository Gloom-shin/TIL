# 사용자 인증 
- 결론 부터 말하자면, 사용자인증을 하지않으면, 다른 repository를 https url 로 가져올수 없다. 
- 물론 SSH인증을 했다면 SSH url 로 가져올 수 있지만, SSH를 하지않으면 이것또한, 가져올 수 없다.

# GitHub CLI 
 - GitHub CLI는 말 그대로 터미널환경에서 깃허브를 사용할 수 있도록 해주는 유틸리티이다.
 - 검색 및 구글링을 해보면, 마이크로소프트가 gitHub를 인수하면서 생긴 기능이라고 한다. 
 - 이말은, 즉 원래는 CLI환경에서 깃허브를 이용하기 어려웠었다는 말이다. 
 - 아무튼 사용하려면, 다운로드를 해야되는 것 같다. 

## GitHub CLI 다운

### 파일 다운로드 명령어
`curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg ` 

- 먼저 `curl`은 리눅스 명령어인데, 사용자 상호 작용 없이 작동하도록 설계된 서버에서 또는 서버로 데이터를 전송하기 위한 명령줄 유틸리티이다.
   -  즉, `curl`을 사용하면, 데이터를 다운로드하거나 업로드 할 수 있다는 것!
- 대부분 리눅스에서 curl 패키지가 설치되어 있지만, 설치되지않은 경우 패키지 매니저인 apt를 통해 설치 해주면 된다. 
- 그 다음 옵션은 curl --help를 통해 나온 것을 가져와 표로 만들었다. 

|옵션| long 타입 옵션| 설명 |
|--|--|--|
|-f| --fail|          Fail silently (no output at all) on HTTP errors|
|-s|--silent  | Silent mode|
|-S| --show-error|  Show error even when -s is used|
|-L| --location|     Follow redirects|

 - 봐도 모르겠다면, 그냥 오류시 대비하기위한 옵션으로만 인지하자(나도 모르겠다..^^)

### 파일 복사
 `sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg`  : 지정한 파일 복사   
 `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main"` : 출력     
 `sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null`   : 내용 기록   

### gh 설치
 1. `sudo apt update`   : 패키지 매니저인 apt의 리스트를 갱신 시키고
 2. `sudo apt install gh`    : gh를 설치한다. 아래 링크를 보시면, gh프로그램에 대해(명령어, 설명서등) 더 자세히 알수 있다.
      - [깃허브 cli 공식 홈페이지](https://cli.github.com/)

## OAuth인증
### OAuth란?
 - 간단하게 말하면 **인증**과 **권한**을 획득하는 것!
    - 인증 : 시스템 접근을 확인(로그인 같은) but, 인증만한다면 openID
    - 권한 : 행위의 권리를 검증  
 - **별도의 회원가입 없이 로그인**할 수 있게 해주는 것이다.
 - 외부 서비스에서도 인증을 가능하게 하고 그 서비스의 API를 이용하게 해주는 것
 
### 인증 방법
 
 - 위에서 언급했던것처럼 `gh`는 Github CLI 프로그램이다.  
   - 하지만, `gh`에 접속하려면 로그인을 해야한다.
   - 로그인을 하지않으면 당연히 github CLI 서비스에 접근할 수 없어, 다른 사람들의 repository를 불러올 수 없다. 
   - 물론 회원가입을 하면 해결이 되지만, 아이디와 비밀번호를 제공해야하는 문제점이 생긴다.
   - 이러한 문제를 해결해 주는 것이 OAuth login이다, 
     - 간편로그인이라고 할수도 있는데 한번 연동해놓으면 아이디와 비밀번호를 제공하지 않고도 로그인이 된다.
 - `gh auth login` 로그인을 시도합니다.  그럼 2지선다 선택지가 4개 나올 텐데 간단한 설정을 하는 것이며, 질문은 아래와 같다.
  >  What accout do you want to log into?  
  > (로그인하려는 계정이 무엇이냐?)    
  > What is your preferred protocol for Git Operations?  
  > (선호하는 프로토콜은 무엇이냐?)  
  > Authenticate Git with your GitHub credentials?  
  > (GitHub 자격 증명으로 Git 인증할것이냐?) -> 필수 Yes  
  > How would you like to authenticate GitHub CLI?  
  > (인증하는 방법은 어떤것이냐?)   

- 4가지 옵션을 선택하면 마지막에 `one-time code`가 나올것이다. 
- 이것은 인증 코드이니, 인증코드를 입력하면 인증 완료가 된다.
