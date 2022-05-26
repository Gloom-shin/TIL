# IntelliJ에서 Gradle 적용하기
 
## 새 프로젝트
- build Tool은 개발 코드를 작성하기전, 개발환경을 세팅하는 단계에서 설정해준다. 
- 그렇기 때문에 보통 `New project` 에서 `build System`항목에 `Gradle`로 설정해주면 된다.
    - 만약 build System`항목에 `Gradle`이 없다면, 읽을 수 없는 상태이거나, 혹은 설치가 되지 않은 것이다.


## Java 프로젝트 😱
 - 일단적으로 IntelliJ 자체적으로 빌드툴을 설정하게 되면, IntelliJ IDEA 엔진에 내장된 자체 빌드툴을 사용한다.
 - 그렇기때문에 `.idea` 폴더의 내용만 있으면 빌드가 된다.
[JetBrain Blog 공식페이지](https://blog.jetbrains.com/upsource/2015/09/09/mysterious-build-system-setting/)
 
### Gradle 프로젝트로 변경하기 ⚠
> IntelliJ에 의존하여 기존 파일을 삭제하고, 덮어씌우는 작업이기에 권장되는 방법은 아니다. 

 - 빌드툴을 내장빌드툴에서 `Gradle`로 바꿔 주려면, 처음 세팅된 값을 제거하고, Gradle 파일로 대체해야 한다.
 1. `.idea` 폴더를 모두 제거하고, `.iml`파일도 모두 제거 해준다.
 2. `Gradle`의 핵심인 `build.gradle`파일과 `settings.gradle`파일을 추가해준다.
 
#### `build.gradle`파일

```
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'  // 최신화 해주면된다.
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

test {
    useJUnitPlatform()
}
```
#### `settings.gradle`파일
```
rootProject.name = '프로젝트이름'
```
- 프로젝트이름에 작업중인 프로젝트이름으로 변경하면된다. 

### Gradle Project로 만들기 
- 위 두 파일을 작성하여 만들면, IntelliJ가 자동으로 `Load Gradle Project` 알림이 뜬다. 
  - 만약 안 뜨더라도 `import Gradle Project`로 하면된다.
- 그럼 새로운빌드를 진행하고, `.idea`폴더를 만들고, 외부라이브러리도 받아오면서 세팅이 완료된다.
- 추가적으로 폴더를 구성하고 싶으면
   -  `src`폴더에 `new` -> `Directory`를 누르면, 기본적으로 생성해줄 `main/java, main/resource, test/java, test/resource` 폴더는 생성할 수 있다.
   -   그리고 기존 java파일과 리소스 파일을 정리하여 넣어주면 된다.
   -  만약 java파일이 아닌 폴더(패키지)로 구분해놨었더라면, `Mark Directory as` -> `source Root`로 설정해주면 된다. 

