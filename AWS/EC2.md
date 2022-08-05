# Amazon EC2란? 
- EC2는 `Elastic Compute Cloud` 의 줄인말로, 탄성적인 클라우드 컴퓨터를 말한다. 
  - 여기서 `Elastic`은 고무밴드란 뜻으로 그만큼 유두리있게, 늘렸다 줄였다해서 사용할 수 있는 것을 말한다.
  - 즉, 클라우드에서 쉽게 확장가능 컴퓨터를 제공한다는 것이다.
- Amazon EC2를 사용하여 원하는 수의 가상 서버를 구축하고 보안 및 네트워킹을 구성하며 스토리지를 관리할 수 있다.

  <br></br>

## 기능 
- 가상 컴퓨터환경인 인스턴스를 만들 수 있다.
- 물론 그 인스턴스는 사전에 구성된 템플릿의 인스턴스도 있으며, 이것을 `AMI(Amazon Machine Images)`이라 불린다.

<img src="https://user-images.githubusercontent.com/104331549/183069218-30a24342-efbb-44cc-abf1-ff9d29b4befb.png" >

- OS가 정해지면 CPU, 메모리, 스토리지, 네트워킹 용량도 다양하게 선택하여 인스턴스를 만들 수 있다.
  <img src="https://user-images.githubusercontent.com/104331549/183069726-4e5bc039-f3c8-4482-a507-7e25af5fc767.png" >

- AWS는 퍼블릭 키를 저장하고 사용자는 개인 키(`.pem`)를 안전한 장소에 보관하는 방식으로, 키페어를 제공한다.
  - 여기서 AWS가 퍼블릭 키인 이유는 정보의 암호화보다는 사용자 인증과 권한에 초점이 맞춰져있기 때문이다.
  - `.pem`파일을 가지고, SSH를 통해 인스턴스 PC에 원격으로 접속하여 제어할 수 있다.
    <img src="https://user-images.githubusercontent.com/104331549/183069742-41685a4c-2f24-4dc7-8ec6-8a59d024484e.png" >
  
  - 원격 접속제어에 관련한 내용은 `인스턴스에 연결` ->`SSH 클라이언트`에 자세히 나와있다.
  - 원격 연결의 키인 `.pem`파일의 권한이 **파일 소유자만** 읽기가능한 상태여야한다.(쓰거나,실행 불가능해야됨)
    <img src="https://user-images.githubusercontent.com/104331549/183072312-175c8a23-d4e2-4c07-b85e-f2309b210e1b.png" >
- 원격접속이 아니라면, AWS에서 Sesstion Manager를 통해 접속 할 수 있다.
### 네트워크 설정 
 - VPC 설정 
 - 서브넷 설정
 - Public IP 자동할당
   - 기본값으론 활성화 되어있어 서버를 닫으면 `IP`값이 변경되는데, 비활성화 하면 고정 `IP`로 사용 할 수 있다.(추가 과금 방생)
 - 방화벽(보안 그룹)
   - 직접 생성 가능
   - 기존에 만들어 놨던 보안 그룹 사용가능
   - **인바운드 보안 그룹 규칙** 
     - 원하는 요청 프로토콜(TCP,UDP등)과 원하는 포트범위(443, 8080등)를 정해 원하는 경로로만 인스턴스에 접근가능하도록 할 수 있다. 
     - 아래에서 보안그룹에대해 따로 다룸.



<br></br>
<br></br>

## Security Group
 - 보안 그룹은 인바운드와 아웃바운드 트래픽을 관리하는 인스턴스의 가상 방화벽을 말한다.
 - 여기서 중요한 것은 **인바운드**와 **아웃바운드**이다.
<img src="https://user-images.githubusercontent.com/104331549/183073571-8f29b997-98f6-4e13-82e1-4000d40a0ca3.png" >

### 인바운드 
- EC2인스턴스로 들어오는 트래픽에 대한 규칙
- 프로토콜, 포트범위, 특정IP를 설정할 수 있다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183075328-4521fd8b-84ff-4ff1-9fd7-98293abf1b67.png" width="65%"></p>

### 아웃바운드 
- EC2인스턴스에서 나가는 트래픽에 대한 규칙
- 따로 설정하지 않으면, 기본값은 모든 트래픽을 허용

### 태그 
 - 내가 원하는 형태의 인바운드와 아웃바운드를 만들었다면, 검색하기 쉽게 태그를 남길 수 가 있다.


## 참고링크 
[AWS EC2란?](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html)
