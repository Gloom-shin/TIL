# Web의 구조
- RDS와 S3를 알기전에, Web의 구조를 먼저 알아야한다.
  - 참고링크 [웹의 역사](https://github.com/Gloom-shin/TIL/blob/93a2a6f615f2430b30ec9c5c60e28bc29c6cd46c/%EC%9D%B8%ED%84%B0%EB%84%B7/WAS%EC%99%80%20WebServer(%EC%9B%B9%EC%9D%98%20%EC%97%AD%EC%82%AC).md)
- 웹의 구조는 아래와 같이, 정적인 페이지를 관리하는 서버와 동적인 페이지를 관리하는 서버가 있다.
- 그리고 회원정보와 같은 동적인 정보를 보관하는 데이터베이스도 있다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183078003-44e8f804-7de0-4076-82e8-3590aadccfc8.png" width="65%"></p>

- 웹 서비스 특성상, 언제 어디서나,접근이 가능하여, 보통 24시간 상시로 서버와 DB가 켜져있는게 대부분이다. 
  - 하지만, 로컬에서 개발하여 서버를 열어주고 DB를 가동하게 되면,우리집 컴퓨터의 전원이 꺼지는 순간, 서버의 데이터는 날아가고, 서비스도 종료된다. 
  - 그렇기 때문에, 내 로컬PC(우리집 컴퓨터)가 아닌 24시간동안 돌아갈 수 있는 컴퓨터를 마련해야하는데, 그것을 AWS의 클라우드 컴퓨터를 빌리는 것이다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183079779-fbba76ea-2d85-419e-b0fd-584a09ae5d4e.png" width="65%"></p>

- 그래서 다시한번 정리해서 구조적으로 보자면 아래와같다. 
  - 만약 로드밸런서`ELB(Elastic Load Balancer)` 가 추가 된다면, EC2 앞이 된다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183081053-549eedf5-bea0-4479-886a-e42ff7b6e5a9.png" width="65%"></p>

<br></br>
<br></br>

# RDS
- Relational Database Service의 약자로 AWS에서 제공하는 관계형 데이터베이스 서비스이다.
- 데이터베이스 엔진마다 제공하는 기능이 조금씩 다르기에 필요와 목적에 맞게 데이터베이스 엔진을 선택하여 효율성을 높일 수 있다.
- 이용시 다양한 관계형 데이터베이스 엔진을 선택할 수 있다.
  - 관계형이 아닌 DB는 NoSQL 데이터베이스 서비스인 DynamoDB를 이용하면된다.
- 생성시, EC2랑 연결할 포트번호에 유의하자.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183083147-2b5f2e10-11e6-439d-b6d5-78f58340c22a.png" width="65%"></p>

<br></br>
<br></br>

# S3
 - Simple Storage Service의 약자로 AWS 제공하는 객체 스토리지 서비스이다.
 - Amazon S3를 사용하여 데이터 레이크, 웹 사이트, 모바일 애플리케이션, 백업 및 복원, 아카이브, 엔터프라이즈 애플리케이션, IoT 디바이스, 빅 데이터 분석 등 다양한 사용 사례에서 원하는 양의 데이터를 저장하고 보호할 수 있다.

## 주요 기능
### 스토리지 클래스 
> 여러 사용 사례에 맞춰 설계된 다양한 스토리지 클래스를 제공
 - 예를 들어
   - 자주 액세스하는 프로덕션 데이터를 `S3 Standard`에 저장
   - 액세스 빈도가 낮은 데이터는 `S3 Standard-IA` 또는 `S3 One Zone-IA`에 저장하여 비용을 절감가능
   - 오랫동안 보관해야 하는 데이터는 `S3 Glacier Flexible Retrieval` 및 `S3 Glacier(빙하라는 뜻) Deep Archive`에 저장할 수 있다.
 - Build파일 생성시, 중요한 정보 및 동적인 변수는 환경변수로 설정하는것이 중요하다.

### 스토리지 관리
 - S3 수명 주기 : 객체의 수명주기를 관리 및 전환 가능
 - S3 객체 잠금 : 삭제 또는 덮어쓰기 방지 기능
 - S3 복제 : 다른 AWS 리전에 있는 하나 이상의 대상 버킷에 복제
 - S3 배치 작업 (추후 배치에 대한 설명언급할 예정)

### 엑세스 관리
> 버킷 및 객체에 대한 접근 확인 및 관리

- S3 퍼블릭 액세스 차단 :
  - S3 버킷과 객체에 대한 **퍼블릭 액세스를 차단**한다.
  - 기본적으로 퍼블릭 액세스 차단 설정은 계정 및 버킷 수준에서 켜져 있다.

- AWS Identity and Access Management(`IAM`) : 
  - AWS 계정용 IAM 사용자를 생성하여 Amazon S3 리소스에 대한 액세스를 관리한다.
  - 예를 들어
    - `IAM`을 Amazon S3와 함께 사용하여 사용자 또는 사용자 그룹이 AWS 계정에 속한 S3 버킷에 대해 접근 유형을 제어할 수 있다.

- 버킷 정책 :
  - `IAM` 기반 정책 언어를 사용하여 버킷과 객체에 대한 리소스사용 권한을 구성

- Amazon S3 액세스 포인트:
  - Amazon S3의 공유 데이터터 집합에 대한 데이터 액세스를 대규모로 관리하기 위해 전용 액세스 정책이 포함된 **네트워크 엔드포인트**


- S3 객체 소유권 :
  - 액세스 제어 목록(ACL)을 활성화/비활성화 할 수 있다.
    - 인증된 사용자에게 개별 버킷 및 객체에 대한 읽기 및 쓰기 권한을 부여한다. 
    - 일반적으로 ACL 대신 액세스 제어를 위해 S3 리소스 기반 정책(버킷 정책 및 액세스 포인트 정책) 또는 IAM 정책을 사용하는 것이 좋다.(ACL 비권장)
    - ACL은 IAM보다 먼저 적용된다.
  - 버킷 소유자는 버킷의 모든 객체를 자동으로 소유하고 완전히 제어할 수 있으며 데이터에 대한 액세스 제어는 정책(default: 비활성화)
  - ACL활성화 하면, 이 버킷의 객체는 다른 AWS 계정에서 소유할 수 있다. 

- Access Analyzer for S3
  - 평가 및 모니터링 분석 

### 데이터 처리
 > 데이터를 변환하고 워크플로를 트리거하여 다양한 다른 처리 작업을 대규모로 자동화할 수 있다.
 - S3 객체 Lambda : 
   - 데이터를 수정 및 처리할 수 있다.
 - 이벤트 알림 :
   -  S3 리소스가 변경되면 `Amazon SNS`와 `Amazon SQS` 및 `AWS Lambda`를 사용하는 워크플로를 트리거한다.
### 스토리지 로깅 및 모니터링
> Amazon S3 리소스가 사용되는 방식을 모니터링하고 제어하는 데 사용할 수 있는 로깅 및 모니터링 도구를 제공
### 분석 및 인사이트 
> 스토리지 사용량을 파악할 수 있는 기능을 제공
### 강력한 내구성
> 데이터 센터를 여럿 만들어 다른 가용 영역에 백업을 해놓은 데이터를 활용하여 높은 가용성과 높은 내구성을 보장한다
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183088802-5c3e699a-f808-45f1-8278-b2629f1ad435.png" width="65%"></p>

<br></br>

## 작동방법
> S3는 버킷과 객체로 저장하는 서비스이다.
 - 객체는 해당 파일을 설명하는 모든 메타데이터
 - 버킷은 객체에 대한 컨테이너

### 동작 순서
> 그래서, 먼저 버킷을 생성해야한다. 
 - 버킷 이름, 리전(지역)지정하여 버킷을 생성한다.
 - Amazon S3에서 객체로 해당 버킷에 데이터를 업로드
 - 모든 객체는 고유한 키를 가지는데,  이 키를 이용하여 원하는 객체를 검색할 수 있다.
 - 또한, 모든 객체는 고유한 URL 주소를 가지고 있어, `http://[버킷의 이름].S3.amazonaws.com/[객체의 키]`의 형태로 접근 가능하다.

### 버킷
 - Amazon S3에 저장된 객체에 대한 컨테이너
 - 버킷에 저장할 수 있는 객체 수에는 제한이 없다.
 - 한 계정에 버킷을 최대 100개까지 포함
### 객체
 - Amazon S3에 저장되는 기본 개체
 - 객체 데이터와 메타데이터로 구성
   - 메타데이터는 객체를 설명하는 `이름-값` 페어의 집합
   - 여기에는 마지막으로 수정한 날짜와 같은 몇 가지 기본 메타데이터 및 Content-Type 같은 표준 HTTP 메타데이터가 포함
 - 버킷 내 모든 객체는 정확히 하나의 고유한 키를 갖는다.
   - 예를들어, `https://DOC-EXAMPLE-BUCKET.s3.us-west-2.amazonaws.com/photos/puppy.jpg` URL에서 
     - `DOC-EXAMPLE-BUCKET`은 버킷의 이름
     - `photos/puppy.jpg`는 키

<br></br>
<br></br>

# AWS Identity and Access Management(IAM)
> AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스이다.
> IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여(권한 있음)된 대상을 제어한다.

 - AWS 계정을 처음 생성할 때는 모든 AWS 서비스 및 리소스에 대한 완전한 접근권한이 있는 통합 인증 ID이다. 
 - 즉, 이 계정이 `root 사용자`가 되는 것이다.
 - 그리고 이계정을 통해 파생되는 각각 권한이 다른 ID를 만들 수 있는데, 그것이 IAM계정이라 한다.
 - 그룹으로 만들어서, 그룹안에 권한을 주는 방식도 있다. 
   - AWS는 권한을 부여하는 정책을 만들어 권한설정에 제공하고 있는데, 
     - 모든 권한을 주는 정책(FullAccess)
     - 읽기만 할 수 있는 정책(ReadOnly)
     - 등등이 있다.
   - 그룹에 태그를 만들어, 검색을 용이하게 할 수 있다.

 - 이때 권한을 부여한 사용자를 만들면, `엑세스 ID`, `비밀 엑세스 키`, `비밀번호`가 발급 된다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183094815-0a594b15-8e51-419b-8d5b-e4cb141280b8.png" width="65%"></p>


## 참고링크
- [AWS S3란?](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/Welcome.html)
- [AWS DynamoDB란?](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Introduction.html)
- [AWS RDS란?](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Welcome.html)
- [AWS IAM 소개](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/introduction.html)
- [IAM 설정 + AWS 계정을 만들고 처음 해야할 일](https://youtu.be/ET87afIo4SQ)