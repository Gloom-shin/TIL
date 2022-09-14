# EC2 접속하기 
> AWS에서 서버를 24시간 배포할 때 사용하기 좋은 EC2 인스턴스를 만들면,   
> EC2의 배포환경을 맞추기위해, 안에 작업을 해줘야되는데, 로컬PC가 아닌 클라우드이기에, 로컬과 접속하는 방식이 다르다.   
> 그래서 이번엔 EC2에 접근 하는 방법에 대해 알아보자 

## 작업 환경
 > 인스턴스 OS는 리눅스이며, 우분투로 작업하였고, 프리티어로 생성하였다. 

# 외부 IP로 접근 하기
 - 인스턴스를 만들면 키페어를 만들지에 대한 옵션이 뜨는데, 이것이 바로 외부에서 접근하기 위한 키를 생성하는 것이다.
   - 키페어 유형은 키가 어떻게 암호화 될것인지 선택하는 것이고
   - 프라이빗 키 파일 형식은 파일의 확장자이다. 
   - `.pem` : OpenSSH와 함께사용
   - `.ppk` : PuTTY와 함께 사용

> 여기서, 나는 `.pem` 확장자를 사용하여 키를 만들었다.
> 
## OpenSSH 사용하여 접근하기



## 직접 접근하기
 - `Session Manager`를 통하여 접근하며, 가장 최근에 생긴 접근 방식이며, 사용하는 방법도 매우 간단한 방법이다. 
 - `Session Manager`는 대화형 원클릭 **브라우저 기반 셸**이나 **AWS CLI**를 통해 Amazon EC2 인스턴스를 관리할 수 있는 완전관리형 AWS Systems Manager 기능이다.
   - 즉, CLI는 AWS CLI를 사용하며
   - 작업하는 Shell 창은 브라우저로 띄어준다는 것이며, 
   - 그 브라우저를 띄우는 방법은 연결 버튼만 누르면 된다. 
### 연결 과정
 - EC2 콘솔 -> 인스턴스
 - 원하는 인스턴스를 체크하고, 연결 클릭!
 - 연결방법메뉴중에 세션 관리자 탭 클릭!
 - 연결 클릭!

### 작업 환경 갖추기
> 하지만, 위방식으로 접근시, 수행할 권한이 없다는 오류가 표시 될 수 있다. (처음엔 대부분 뜬다.)  
> 이것은 해결하려면 세션을 시작할 수 있도록 업데이트를 해야한다.
- 일단, 세션관리자 권한을 주는 가장 일반적인 방법은 IAM 정책을 생성하는 것이다.
  - IAM은 간단하게 부계정을 판다고 생각하시면 된다.(자세한 내용은 추후에)
- 이 IAM계정에는 적어도 세션관리자 콘솔과 AWS CLI이나 AWS EC2 콘솔 이렇게 사용할 수 있어야 한다. 
  - 그래서 이 `세션관리자 +AWS CLI` 혹은 `EC2` 혹은 `EC2 + 세션관리자 + AWS CLI`로 총 3가지 방식으로 권한을 준 계정을 만들 수 있다.
  - 하지만, 귀찮으니, 마지막 3가지권한을 다 주는 방식만 보도록 하자.

### 작업 순서 
1. 먼저 `IAM`에 들어가서 정책을 클릭
 <img src = "https://user-images.githubusercontent.com/104331549/188369885-52e5737e-1006-4302-ae5e-29fe7d980a2e.png">

2. 정책 화면에서 정책 생성 클릭
<img src = "https://user-images.githubusercontent.com/104331549/188369936-0b53d34f-16b5-4caf-b417-cd7b94416c19.png">

3. 정책 생성 화면에서 시각적 편집기가 아닌 `Json`으로 클릭
<img src = "https://user-images.githubusercontent.com/104331549/188369898-3d7fde50-844a-4ff3-b3b5-7c1107202d9a.png">

4. 아래 코드 입력

```Json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:SendCommand" 
            ],
            "Resource": [
                "arn:aws:ec2:region:account-id:instance/instance-id",
                "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell" 
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true" 
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceInformation",
                "ssm:DescribeInstanceProperties",
                "ec2:DescribeInstances"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:TerminateSession",
                "ssm:ResumeSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        },
//        {
//            "Effect": "Allow",
//            "Action": [
//                "kms:GenerateDataKey" 
//            ],
//            "Resource": "key-name"
//        }
    ]
}
```


- 가장 바르게 세션관리자권한을 주는 방법가이드 [AWS 공식홈페이지](https://docs.aws.amazon.com/ko_kr/systems-manager/latest/userguide/getting-started-restrict-access-quickstart.html)
- 세션이 시작된 후에는 bash 명령어를 실행시켜 작업을 시작하면 된다.

> 직접 접근하는 방식은 아직 공부가 더 필요해 보인다.