
# EC2 인스턴스만들때 많이 사용하는 명령어
> 통상적으로 배포를 할때 클라우드로 서버를 만들어 배포를 많이하게 되는데, 배포하는 서버의 운영체제는 Linux(Ubuntu)로 배포를 많이 하게 된다.  
> 하지만, 매번 배포할때마다, 자주 사용하게되는 명령어를 매번 찾게되는게 번거로워 이번 기회에 정리해두고자 한다. 

## 목차 
 - 쉘이란? 
  -

<br></br>

# sudo bash 
 - 리눅스 운영체제에 접속하면, 가장 먼저 치는 명령어가 `sudo bash`이다. 
 - 처음에는 아무 생각없이 쳤었지만, 적어도 `bash`가 무엇인지는 알아볼 필요가 있는 것 같아 한번 찾아 보고자한다.

## 쉘이란? 
> bash를 알기전 쉘에 대해 알고 있어야 한다.
 - 쉘은 쉽게 말해 **명령을 받는 컴퓨터 프로그램**이다.
   - 즉, 명령어만 입력할 수 있다는 말이며, 명령어를 입력하면 그 명령을 해석하고 처리를 위해 운영 체제에 전달한다.
 - 결론적으로 사용자가 운영 체제와 상호 작용할 수 있도록 사용자와 운영 체제 간의 인터페이스 역할을 한다.

<div align="center"><img src="https://user-images.githubusercontent.com/104331549/194840334-d13de820-dbb2-4e12-b6ce-4670f942ea8c.png" width="70%">
</div> 

 - 리눅스와 같이 `CLI`가지는 운영체제의 경우 `shell`로 작업을 해야한다.
 - 이번엔 리눅스만 다룰꺼기 때문에, 리눅스 위주로 알아보자
### 리눅스 쉘의 종류 
 - 오리지날은 `Steve Bourne`의 Bourne shell인 **"sh"** 이다.
 - 그외에 대표적으로 자주 사용되는 shell은 bash, ksh, tcsh, zsh 정도가 있다. 
   - BASH (Bourne-Again Shell) [/bin/bash] : 일반적으로 가장 흔하게 사용되는 쉘이다. sh 본쉘과 호환되기 때문에 대부분 sh와 bash에서 모두 작동된다.
   - CSH (C Shell) [/bin/sh] : C 언어와 유사한 문법을 가지고 있다. 유닉스의 기본 쉘이다.
   - KSH (Korn Shell) [/bin/ksh] : 초심자를 위해 표준 환경이 적용되어 있는 Bourne쉘의 슈퍼셋이다. 유닉스 지식을 가지고 있는 사람들에게 인정받고 있는 쉘이다.
   - TCSH [/bin/tcsh] : 일반적인 C 쉘이며 사용자 중심이고 속도가 빠르다.
 - 대표적으로 유닉스 운영체제(MacOS)가 아니라면, bash나 sh를 쓴다고 보면된다.

### 사용가능한 쉘
 - Linux 시스템에서 사용 가능한 모든 쉘은 `/etc/shells` 파일에 나열된다. 
   - `cat /etc/shells` cat 명령을 통해 내용을 확인할 수 있다.  
   - ec2는 아래와 같이 기본적으로 사용가능 한 쉘을 확인해본 것이다.(버전이나, 사양에 따라 달라질 수 있다.)
     <img src="https://user-images.githubusercontent.com/104331549/194861340-4a19c711-e66e-482e-8529-585bdf21c818.png">
   
 - 현재 사용중인 쉘 확인하는 방법은 `env | grep SHELL` 혹은 `echo $SHELL` 으로 확인할 수 있다.  
   - 여기서 중요한 것은 리눅스는 대소문자를 구분하니, 꼭 `SHELL`로 입력해줘야한다.
   - 그럼 아래와 같이, `/bin/[현재 사용중인 쉘]` 이 출력된다.  
       <img src="https://user-images.githubusercontent.com/104331549/194862915-e6c2e8bf-3006-414d-a05f-07563b9b7849.png">


<br></br>
<br></br>

# 리눅스 bash shell 명령어 
> 리눅스 운영체제에서 배포를 하는 것이라면 간단하게 리눅스 명령어(정확히는 bash shell 명령어)를 알아야함을 느꼈다.  
> 그래서 자주쓰이는 것 위주로 다뤄보고자 한다.  



## bash shell 명령어
 - man : command, system call, function 등 다양한 리눅스 명령어의 사용법을 확인
 - passwd : 로그인한 사용자 ID의 암호변경
 - pwd : 현재 디렉토리 위치
 - cd : 디렉토리 이동
 - mkdir : 디렉토리 생성 
 - ls : 현재 위치한 디렉토리에 있는 폴더와 파일 확인
 - cat : v파일 보기 
 - rm : 파일 및 폴더 삭제
   - `-r` 옵션 : 하위 디렉토리를 포함한 모든 파일 삭제
   - `-f` 옵션 : 강제로 파일이나 디렉토리 삭제
 - mv : 파일의 이름을 변경하거나 디렉토리 경로 변경
 - clear : 콘솔에 있는 명령어를 모두 지운다.
 - history : 이전에 사용된 명령어 이력 확인
 - find : 폴더 또는 파일 검색 
   -  `-name "파일명"`입력 시 현재 폴더에서 파일명을 가진 파일 또는 폴더를 검색한다. 
 - grep : 파일 또는 텍스트에서 특정 키워드가 포함된 줄 출력
   - `grep [옵션] [패턴] [파일명]` 형식으로 사용한다.  
   - `-i` 옵션 :  Insensitively하게(대소문자 구분 없이) 찾는다.
   - `-w` 옵션 : 정확히 그 단어만 찾기
   - `-v` 옵션 : 특정 패턴 제외한 결과 출력
   - `-E` 옵션 : 정규 표현식 사용

 - ps : 현재 실행중인 Process 확인
   - `pipe`랑 같이 쓰인다.
 - export : 환경변수 설정
 - chmod : 파일 권한을 변경
   - 읽기권한 4, 쓰기 권한 2,실행권한 1 => 모든권한 7로 적용할 수 있다.
 - nohup : 터미널 종료 후에도 계속 작업이 유지되도록 백그라운드로 process를 실행
 - curl : 웹 서버에 요청을 보냄(주로 웹으로부터 파일을 다운로드받을 때 사용)
 
## Pipe와 리다이렉션
 > 리눅스 쉘 명령어를 보다보면,  `<`, `>` , `|` 과 같은 기호를 접하게 된다. 
 > 이 기호들은 단순히 하나의 명령어만 실행할 수 있는 CLI에 복잡한 작업을 할 수 있게 도와준다.
### standard stream
 - pipe와 리다이렉션에 들어가기 앞써, command로 실행되는 process는 3가지 스트림을 가지고 있다. 
   - 표준 입력 스트림(standard input stream)
   - 표준 출력 스트림(standard output stream)
   - 오류 출력 스트림(standard error stream)
 - 이 모든 스트림은 일반적인 plain text로 console 에 출력된다. 
 - 리다이렉션은 이런 스트림의 흐름을 바꿔주는 기능을 한다.
 - 파이프는 두 프로세스 사이에서 한 프로세스의 출력 스트림이 또 다른 프로세스의 입력 스트림으로 사용될 때 쓰인다.
### Redirection: > (write to file)
- `>`를 사용하면 stdout의 내용을 파일에 적을 수 있다.
- ex) `ls > files.txt`
  - ls로 출력되는 표준 출력 스트림의 방향을 files.txt로 바꿔줌으로써, 현재 위치의 파일리스트가 아닌 files.txt에서 ls로 출력되는 결과가 나온다.
### Redirection: < (read from file)
- `<`를 사용하면 file의 내용을 읽어올 수 있다.
- ex) `head < files.txt`
  - files.txt의 파일 내용이 head라는 파일의 처음부터 10라인 까지 출력해주는 명령으로 된다.


### Redirection: | (Pipelining)
 - `|`를 사용하여 command 를 연결할 수 있다. 
 - 즉, 다중 명령어작업을 실행 할 수 있다. 
 - ex)` ls | grep files.txt`
   - ls 명령을 통한 출력 내용이 grep 명령의 입력 스트림으로 들어 간다.

# 실제 사용하게되는 명령어 
## JDK 세팅 
### 매니저 체크
- `sudo apt update`
### JDK 설치
- `sudo apt install openjdk-11-jre-headless`
### 버전 확인 
 - `java --version`

## git 
### git clone
 - `git clone [복사할 깃허브 경로]`
### 프로젝트 빌드
 - `./gradlew build`
## MySQL 세팅
### mysql 설치
 - `sudo apt install mysql-server`
### mysql 설치 확인
 - `dpkg -l | grep mysql-server`
 - mysql 직접 실행 :`sudo mysql -u root -p` 
### database 만들기
 - `CREATE DATABASE {DB이름};`

### mysql 사용자 만들기 
 - 사용자 생성 :`create user '[사용자명]'@'[접근가능한 곳]' identified by '[비밀번호]';` 
   - ex) `CREATE USER 'testUser'@'localhost' IDENTIFIED BY '1234';`
   - `localhost`에서 접속 가능한 testUser 라는 사용자 명을 만들었으며, 비밀번호는 1234 이다.
 - 권한 부여 : `grant all privileges on [데이터베이스명].[테이블명] to [사용자명]@'[접근가능한 곳]';`  
   - ex) `mysql> GRANT ALL PRIVILEGES ON testDB.* TO 'testUser'@'localhost';`
   - testUser는 testDB에 모든 테이블에 대해 모든 권한이 생긴다.
 - 권한 적용 : `FLUSH PRIVILEGES;`

## 배포
### 백그라운드에서 실행 
 - `nohup java -jav [실행파일]`
### 백그라운드에서 log 없이 실행 
 - `nohup java -jav [실행파일] 1> /dev/null 2>&1 &`
### 실행중인 PID 확인하기 
 - 리눅스 명령어
 - `sudo lsof -t -i:8080 `
   - 8080 포트로 실행하고 있는 프로세스 아이디를 찾음
 - `sudo kill -9 [프로세스ID]`
   - 해당 PID 강제종료


## 참고자료
 - [리눅스 쉘 기본명령어 이해 및 실습](https://www.fun-coding.org/linux_basic2.html)
 - [유용한 쉘 명령어 (Shell commands) 모음](https://bioinfoblog.tistory.com/11)
 - [MySQL Database 생성 및 권한부여](https://devdhjo.github.io/mysql/2020/01/29/database-mysql-002.html)