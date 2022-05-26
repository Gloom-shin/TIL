# IntelliJ에서 Gradle 적용하기
 
## 새 프로젝트
- build Tool은 개발 코드를 작성하기전, 개발환경을 세팅하는 단계에서 설정해준다. 
- 그렇기 때문에 보통 `New project` 에서 `build System`항목에 `Gradle`로 설정해주면 된다.
    - 만약 build System`항목에 `Gradle`이 없다면, 읽을 수 없는 상태이거나, 혹은 설치가 되지 않은 것이다.


## Java 프로젝트 
 - 일단적으로 IntelliJ 자체적으로 빌드툴을 설정하게 되면, IntelliJ IDEA 엔진에 내장된 자체 빌드툴을 사용한다.
 - 그렇기때문에 `.idea` 폴더의 내용만 있으면 빌드가 된다.
[JetBrain Blog 공식페이지](https://blog.jetbrains.com/upsource/2015/09/09/mysterious-build-system-setting/)
 
### Gradle 프로젝트로 변경하기
 - 빌드툴을 내장빌드툴에서 `Gradle`로 바꿔 주려면, 처음 세팅된 값을 제거하고, Gradle 파일로 대체해야 한다.
 1. `.idea` 폴더를 모두 제거하고, `.iml`파일도 모두 제거 해준다.
 2. `Gradle`의 핵심인 `build.gradle`파일과 `settings.gradle`파일을 추가해준다.
 
