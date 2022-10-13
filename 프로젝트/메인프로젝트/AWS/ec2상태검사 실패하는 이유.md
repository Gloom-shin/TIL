
> 작성 날짜 : 2022.10.13  
> 사용한 스펙    
> AWS EC2 Free Tier(t2.micro)  
> Spring Boot 2.7.3  
> java 11  
> gradle-7.5

# ec2 상태검사 실패
 - 프리티어EC2를 사용하던 도중 서버를 실행시키면, 아래와 같이 상태검사가 실패하는 경우가 종종있다.
 - 이렇게 상태 검사를 실패하게되면, 서버가 정상작동하지 못하게된다. (인스턴스를 중지했다가 다시 켜야됨)
![1](https://user-images.githubusercontent.com/104331549/195498605-2fce344b-eaa2-4b2a-9979-bb8f5787d231.png)

> 그럼 대체 상태검사 실패가 왜 이러나는 것일까? 

<br>

# 상태검사 실패 이유
 - AWS에서 상태검사는 2가지로 제공해준다. 
   - 시스템 상태검사, 인스턴스 상태검사
![상태검사이미지](https://user-images.githubusercontent.com/104331549/195501776-c211fd50-8948-4085-9e85-64d57ba4b965.png)
 - 이러한 상태검사는 1분마다 실행되며, 모든 하나라도 검사결과가 실패인 경우, 인스턴스 전체가 손상되었다고 표시되여, 상태검사가 실패했다고 뜬다.
 - 이러한 상태검사 기능은 EC2에 내장된 기능으로 삭제하거나 비활성화 할 수 없다.

## 시스템 상태 검사
 - 인스턴스가 실행되는 기본 호스트에서 문제, 근본적인 문제를 탐지한다.
 - 네트워크, 시스템 전원 중단, 물리적 호스트의 소프트웨어 문제등 
 - 대부분 EC2를 운영하는 AWS측에서 문제가 생겼을때 발생하는 상태검사이다.

## 인스턴스 상태 검사
 - 개별 인스턴스에 대한 소프트웨어 및 네트워크 구성에 대한 문제를 탐지한다.
 - Amazon EC2는 네트워크 인터페이스(NIC)로 주소 확인 프로토콜(ARP)을 전송하여 인스턴스의 상태를 확인한다.()
 - 문제가 발생하여 복구 할 시 사용자가 직접 문제를 찾아야한다. 
   - 일반적으로 인스턴스를 재부팅하거나, 구성을 변경하는 방법으로 해결한다.
 - 또한, 대부분의 실패하는 이유는 아래와 같다.
### 인스턴스 상태검사 실패 원인
 - 운영 체제 부팅 실패
 - 올바른 볼륨 탑재 실패
 - CPU 및 메모리 소진
 - 커널 패닉(호환되지않는 커널)
 - 네트워크가 작동하지 않음

> 해결방법중 일부는 인스턴스를 재부팅이 아닌 중지하고 다시 시작해야되는 경우도 있는데, 이경우 인스턴스 스토어에 있는 데이터가 모두 손실된다.  
> 또한, IP도 동적 IP풀에 다시 릴리스 된다. 
> 자세한 내용은 [링크 참조](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/Stop_Start.html#instance_stop)

# 내가 발생한 문제들 
 > 내가 발생한 EC2 오류는 대부분 CPU 및 메모리에 관련된 문제들로 인해 발생했다.
## 메모리 부족
 - 프리티어에서 EC2 안에서 `Jar` 파일을 만들기 위해서,`gradlew build`를 하지만, EC2가 그대로 멈춰버린다. 
 - 즉, 메모리 1GB의 양으로는 인스턴스의 기본적인 프로그램을 제외한 나머지 메모리론 빌드조차 하기 어렵다는 것이다.
   - 그러면 실제 Spring Boot를 실행해서도 문제가 많을 것으로 보인다.
 - 게다가 EC2 메모리 및 디스크 지표는 기본적으로 CloudWatch로 전송되지 않는다.(즉, 왠만한 방법으로는 모니터링하기 어렵다.)
   - 이부분은 CloudWatch 에이전트를 사용하여 해결할 수 있다.(추후 확인해볼 예정)

### 해결방법
> AWS 공식홈페이지에서는 크게 2가지 해결방법을 제시해준다. 

#### 첫번째, 성능 좋은 인스턴스유형으로 업그레이드 하는 것이다.
   - 물론 비용이 든다. 또한 호환되는 인스턴스 유형으로만 업그레이드할 수 있다.
   - [참고링크](https://aws.amazon.com/ko/premiumsupport/knowledge-center/resize-instance/)

#### 두번째, Swap
 - 하드공간(SSD 혹은 HDD)을 RAM 공간 처럼 쓸 수 있게, 가상 메모리로 전환시켜 메모리공간을 늘리는 방법이다.
   - 이를 `Swapping`이라고 한다.
![스왑공간 권장크기](https://user-images.githubusercontent.com/104331549/195522358-39afe3c7-b690-4b8b-8a99-1360af4f33dc.png)

#### 사용법
> Swapping 방법도 크게 2가지가 있다. 하드디스크에 파티션을 만들어 할당하는 방법과, 스왑파일을 만들어 할당하는 방법 

1. 스왑 공간은 물리적 RAM의 2배와 동일한 것이 가장 좋으며, 최소한의 크기는 32MB보단 커야된다.
  이 예제 dd 명령에서 스왑 파일은 4GB(128MB x 32)입니다.

$ sudo dd if=/dev/zero of=/swapfile bs=128M count=32
2.    스왑 파일의 읽기 및 쓰기 권한을 업데이트합니다.

$ sudo chmod 600 /swapfile
3.    Linux 스왑 영역을 설정합니다.

$ sudo mkswap /swapfile
4.    스왑 공간에 스왑 파일을 추가하여 스왑 파일을 즉시 사용할 수 있도록 합니다.

$ sudo swapon /swapfile
5.    프로시저가 성공적인지 확인합니다.

$ sudo swapon -s
6.    /etc/fstab 파일을 편집하여 부팅 시 스왑 파일을 시작합니다.

편집기에서 파일을 엽니다.

$ sudo vi /etc/fstab
파일 끝에 다음 줄을 새로 추가하고 파일을 저장한 다음 종료합니다.

/swapfile swap swap defaults 0 0


## 높은 CPU 사용률
 - 메모리 말고도, CPU사용량이 100%에 가까운 경우 인스턴스에 커널을 실행하기에 충분한 컴퓨팅 파워가 없을 수 있다.
   - 이럴때, 사용되는 것이 CPU 크레딧인데, 사용할 크레딧이 부족하면,  인스턴스 리소스의 과다 사용으로 인해 상태검사가 실패한다.
   - 크레딧 관련 자세한 사항은 [링크참조](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html)
 - 아무튼, 크레딧은 AWS에서 인스턴스를 관리하기위해 만들어놓은 수단같은 것이다. 결국 CPU사용량이 많아지면 실패한다는 것인데, 이것 또한 여러가지 해결책이 있다.
   - 블록 디바이스 오류, 소프트웨어 버그 또는 커널 패닉으로 인해 비정상적인 CPU 사용량 스파이크가 발생할 수 있다.
#### CPU 증가해주는 인스턴스유형으로 업그레이드
 - 메모리 Upgrade 같은 맥락이다.

#### 개인적인 솔루션 
 - Spring Boot로 만든 애플리케이션의 CPU 점유율을 낮추는 방법이다. 
 - 보통 톰캣프로세스가 CPU를 많이 사용하게되는데, 서블릿 즉, 쓰레드를 사용하기 때문이지 않을까 싶다. 
   - 톰캣이 관리하는 `GC`나 혹은 HTTP요청/응답을 관리하는 방법을 알아봐야겠다. 
 - 또한, 번외로 `log`와 같은 정보도 최소화하여 최대한 저장공간의 부담을 줄이는 방법을 알아봐야겠다.

### 참고자료
 - [AWS공식 홈페이지](https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-linux-status-check-failure/)
 - [하드 디스크의 파티션](https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-memory-partition-hard-drive/)
 - [스왑 파일](https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-memory-swap-file/)