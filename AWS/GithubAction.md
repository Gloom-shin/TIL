# 들어가기전에 
> 최근에 생긴 기술인 CI/CD시장에 지각변동을 일으키고 있다는 `GitHubAction`은 대체 뭘하는 것 일까? 
> 그리고 Jenkins, Circle CI, Travis CI와 같은 기존에 유수의 서비스 보다 뭐가 더 좋은 것 일까?
> 자동배포화? 대체 언제 어떻게, 왜 쓰는지를 알기 위해서는
> 먼저 CI와 CD에 대해 알아야한다.

# CI 와 CD
 - `CI`는 `Continuous Integration`의 약자이며, 지속적인 통합을 뜻하며,
 - `CD`는 `Continuous Deployment`의 약지이며, 지속적인 배포를 말한다.
 - 흔히, DevOps 엔지니어의 핵심 업무라고 한다. 
 - 이 CI와 CD를 이해하려면, 프로젝트 기획부터 개발, 배포까지의 과정에 대한 어느정도 지식을 가지고 있어야 이해하기 쉽다.

<img src="https://user-images.githubusercontent.com/104331549/183407499-38e6613c-feb5-4cbb-9a91-52258c541922.png">

## CI
- 정기적으로 짧은 시간에 빌드하고 테스트, 병합하는 것을 뜻한다
- 상대적으로 짧은 시간동안 병합하므로, 개발자들 간의 코드 충돌을 피할 수 있다.
- 단점이라고 한다면 자주 병합하므로 번거로울 수 있다는 점.

### 과거의 개발통합
 - 최종 목표에 도달하기위해, 각자 업무를 나눠 해당 파트를 다 구현해 오는 방식
 - 각자 작업을 진행하고 마지막에 취합하는 방식
    - 업무를 분담하여 진행하기에, 속도가 빠를 수는 있으나,병합하는 과정에서 충돌이 날 수 있으며, 
    - 테스트 과정에서도 생각보다 많이 복잡해진다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183583421-e9450bbc-3ba2-43d4-be23-6cf4fbddc2aa.png" width="65%"></p>
 


### 현재의 개발 통합
 - 한번에 완성하기보다는, 자주 수정해가는 방식이라 보면 된다.
 - 단위별로 개발을 진행하고, 통합(`Integration`) 및 테스트를 하면서 병합하는 방식
   - 중간 중간 테스트를 만들어 성공유무를 파악할 수 있다.
 - 테스트과정이 매우 중요해졌다.
   - 단위 테스트
   - 통합 테스트
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183583434-31274ae2-09a8-474b-a94e-df00d3b2e4ce.png" width="65%"></p>

<br></br>


## CD
- 서비스 배포의 자동화라 할 수 있다.
  - CI의 연장선이라고 볼 수 있다.
- 빌드, 테스트, 배포를 자동화하기 때문에 번거롭게 개발자가 개입할 필요 없어 비용 줄일 수 있고, 사람에 의한 실수를 방지할 수 있어서 위험성을 낮출 수 있다.
- 또한, 자동적으로 빌드 및 테스트를 거치고 배포를 하기 때문에 신뢰성을 보장한 서비스를 제공할 수 있다.
  
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/183585572-0085f4c3-f8af-41e4-88a1-ec4a84646cdc.png" width="65%"></p>



## CI와 CD를 도와 주는 도구
 - Jenkins(제일 유명함) 
 - Circle CI
 - Travis CI
 - Atlassian Bamboo 
 - **GitHubAction**

<br></br>
<br></br>

# GitHubAction
- 비교적 최근에 추가된 CI/CD 툴 서비스 이다.
- 자동으로 코드 저장소에서 어떤 이벤트(event)가 발생했을 때 특정 작업이 일어나게 하거나 주기적으로 어떤 작업들을 반복해서 실행시킬 수도 있다.
  - 누군가 코드 저장소에 `PullRequest`를 생성하게 되면 `GitHubActions`를 통해 해당 코드 변경분에 문제가 없는지 각종 검사를 진행할 수 있다.
  - 어떤 새로운 코드가 메인(main) 브랜치에 유입(push)되면 `GitHubActions`를 통해 소프트웨어를 빌드(build)하고 상용 서버에 배포(deploy)할 수도 있다.
- 매일 밤 특정 시각에 그날 하루에 대한 통계 데이터를 수집시킬 수도 있다.

## Workflows
 - `GitHubActions`에서 가장 상위 개념으로, 쉽게 말해 자동화할 수있는 작업 과정이라고 볼 수 있다.
 - `.github/workflows` 폴더 아래에 위치한 `.yaml` 파일로 설정
 - 하나의 저장소에는 여러 개의 워크플로우, 즉, 여러 `.yaml`파일이 올 수 있다.
   - 최대 20개까지 등록 가능

## 워크플로우 .ymal파일 구성 
 - `.ymal`은 크게 2가지를 정의해야한다. 

<br></br>

### 첫번째, on속성
 - 해당 워크플로우가 언제 실행되는지를 정의
 - 아래는 코드 저장소의 main 브랜치에 push 이벤트가 발생할 때마다, 워크플로우를 실행 시켜준다는 코드이다. 
#### push, PR 
 - 코드 저장소의 main 브랜치에 Pull_request 이벤트가 발생해도 워크플로우를 실행 시켜준다.
   - 하지만, `PR`을 보내는 방법에는 2가지가 있다. 
     1. 저장소의 푸시 권한이 있는 사람이 해당 저장소의 원격 브랜치에서 `PR`
     2. 저장소를 `Fork`한 사람이 포크한 저장소의 브랜치에서 UpStream 저장소로 `PR`
   - 1번의 경우는 상관없지만, 2번의 경우 **권한문제가 발생**하기 때문에, 유의해서 사용해야한다.
```yaml
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
```


#### schedule
 - 매일 원하는 시간에 워크플로우를 실행할 수도 있다.
 - `cron`은 주기를 말한다. 총 5자리로 표현한다.
```yaml
mm HH DD MM DW
 \  \  \  \  \__ Days of week
  \  \  \  \____ Months
   \  \  \______ Days
    \  \________ Hours
     \__________ Minutes
```

  <p align="center"><img src="https://user-images.githubusercontent.com/104331549/183626927-fbbc7e64-7068-442c-a5dd-bfa3b524ee61.png" width="65%"></p>


- 표를 봤을때, **매일 자정에 워크플로우를 실행** 시키기 위해선, 분과 시간은 0 이고, 일 개월, 요일은 모두 포함한다.

- 매일 자정 실행
```yaml
  on:
    schedule:
     - cron: "0 0 * * *" 
```
- 매일인도에서 3:00AM에 빌드
```yaml
  on:
    schedules:
    - cron: "30 21 * * Sun-Thu"
      displayName: M-F 3:00 AM (UTC + 5:30) India daily build
```

<br></br>
<br></br>

## 두번째, Jobs속성
 - 독립된 가상 머신(machine) 또는 컨테이너(container)에서 돌아가는 하나의 처리 단위를 의미
 - 하나의 작업을 필수로 있어야하며, 여러개의 작업으로 구성할 수도 있다.
   - 다른 `Jobs`에 의존 관계를 가질 수 도 있다.
 - 하나의 `Jobs` 안에는 `build`로 시작하며, 필수로 들어가야하는 `runs-on` 속성이 있고, 
 - 그외 여러 `steps`으로 구성되고, 가상 환경의 인스턴스에서 실행된다.

```yaml
obs:
  build:

    runs-on: ubuntu-latest

    steps:
//이하 생략
```

### Step
 - 작업(job)이 하나 이상의 Task로 모델링되는데, 이 Task들은 순차적으로 `steps`에 맞춰 실행된다.
 - 작업 단계는 단순한 커맨드(`command`)나 스크립트(`script`)가 될 수도 있고,
   - `command`와 `script` 사용할 때는 `run` 속성을 사용
 - 좀더 복잡한 액션(`action`)이라고 하는 명령도 있다.
   - `action`을 사용할 때는 `uses` 속성을 사용
<p align="center"><img src="" width="65%"></p>

```yaml
steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    - name: Run chmod to make gradlew executable
      run: chmod +x ./gradlew
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
```
> 아직 자세히까진 들어갈 순없지만, GitHubAction이 무엇인지, 그리고 무슨 역할을 하는지
> 또, .github/workflows/gradle.yaml에는 어떤게 작성되는지까지 감은 잡도록하자!


## 참고 자료 
- [GitHub Actions의 pull_request](https://blog.outsider.ne.kr/1541)
- [CI/CD개념정리](https://memostack.tistory.com/73)
- [schedules](https://docs.microsoft.com/ko-kr/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml)
- [GitHubAction](https://zzsza.github.io/development/2020/06/06/github-action/)