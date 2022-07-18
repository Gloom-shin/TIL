# Spring Rest Docs
> API 문서로 Spring Rest Docs를 사용하는 이유는 
> 이전 글인 API문서화가 필요한 이유를 먼저 보고오는 것을 추천한다.

## API 문서 생성 흐름

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/179381603-c66f9506-d54b-4357-a35b-61e6e28a8de5.png" width="80%"></p>

### 1. 테스트 코드 작성 
> Spring Rest Docs는 테스트코드를 기반으로 만들어지기 때문에, 테스트 코드를 작성부터 해야된다.

 - 슬라이스 테스트 코드 작성
   - API 문서 특성상, 메소드 하나의 기능을 테스트하기보다는, 한 요청에 대한 API 테스트를 해야하기에, 슬라이스 단위의 테스트 코드를 작성해야된다.
   - 요청처리는 Controller에서 하기에, Controller의 슬라이스 테스트를 작성한다.
 - API 스펙 정보 코드 작성
   - 테스트 코드만으로는 문서 정보를 표현하기 부족하다. 
   - Controller에 정의 되어 있는 API 스펙 정보(Request Body, Response Body, Query Parameter 등)를 코드로 작성

### 2. 테스트 업무 실행
 - 작성된 슬라이스 테스트 코드를 실행
   - 하나의 테스트 클래스를 실행 시켜도 되지만,
   - 일반적으로 Gradle의 빌드 task중 `test task`를 실행 시켜서 API 문서 스니핏(snippet)을 일괄 생성한다.
 - 테스트 실행 결과가 `passed`이면 다음 작업을 진행
   - `failed`이면 문제 해결하기위해 테스트 케이스 수정

### 3. API 문서 스니핏(.adoc파일)생성
 - 테스트가 `passed`되면 테스트 코드에 포함된 API 스펙정보 코드를 기반으로 API 문서 스니핏이(`.adoc`)확장자를 가진 파일로 생성된다.
   - 여기서 스니핏(snippet)은 일반적으로 코드의 일부 조각을 의미하는 경우가 많은데, 여기서는 **문서의 일부 조각**을 의미한다.

 - 각 테스트 케이스당 하나의 스니핏이 생성

### 4. API 문서 생성 
 - 생성된 API 스니핏을 모아서 하나의 API문서로 생성

### 5. API 문서를 HTML로 변환
 - 생성된 API 문서를 HTML 파일로 변환

<br></br>
<br></br>

## Spring Rest Docs 세팅
###  build.gradle 설정
- `.adoc` 파일 확장자를 가지는 AsciiDoc 문서를 생성해주는 Asciidoctor 플러그인 추가 
- `ext`의 `set()`메서드를 통해 API 문서 스니핏이 생성될 경로 지정
- AsciiDoctor에서 사용되는 의존 그룹을 지정
  - :asciidoctor task가 실행되면 내부적으로 지정한 `asciidoctorExtensions`라는 그룹을 지정
- 의존 라이브러리 추가
- Gradle의 `:test task` 실행 시, API 문서 생성 스니핏 디렉토리 경로를 설정
- Gradle의 `:asciidoctor task` 실행 시, Asciidoctor 기능을 사용하기 위해 :asciidoctor task에 asciidoctorExtensions 을 설정
- `:build task` 실행 전에 실행되는 task입니다. `:copyDocument task`가 수행되면 index.html 파일이 src/main/resources/static/docs 에 copy 되며, copy된 index.html 파일은 API 문서를 파일 형태로 외부에 제공하기 위한 용도로 사용할 수 있습니다.
  - :asciidoctor task가 실행된 후에 task가 실행 되도록 의존성을 설정 
  - `build/docs/asciidoc/` 경로에 생성되는 index.html을 copy
  - `src/main/resources/static/docs` 경로로 index.html을 추가
- `:build task`가 실행되기 전에 `:copyDocument task`가 먼저 수행 되도록 합니다.
- 애플리케이션 실행 파일이 생성하는 `:bootJar task` 설정
  - `:bootJar task` 실행 전에 `:copyDocument task`가 실행 되도록 의존성을 설정.
  - Asciidoctor 실행으로 생성되는 index.html 파일을 jar 파일 안에 추가
    - jar 파일에 index.html을 추가해 줌으로써 웹 브라우저에서 접속
    - `http://localhost:8080/docs/index.html` 접속하여 API 문서 확인 가능
    - 다른 방식으로도, API 문서 확인 가능
```groovy
plugins {
	id 'org.springframework.boot' version '2.7.1'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id "org.asciidoctor.jvm.convert" version "3.3.2"    // (1)
	id 'java'
}

group = 'com.codestates'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

// (2)
ext {
	set('snippetsDir', file("build/generated-snippets"))
}

// (3)
configurations {
	asciidoctorExtensions
}

dependencies {
       // (4)
	testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
  
  // (5) 
	asciidoctorExtensions 'org.springframework.restdocs:spring-restdocs-asciidoctor'

	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-validation'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	implementation 'org.mapstruct:mapstruct:1.5.1.Final'
	annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.1.Final'
	implementation 'org.springframework.boot:spring-boot-starter-mail'

	implementation 'com.google.code.gson:gson'
}

// (6)
tasks.named('test') {
	outputs.dir snippetsDir
	useJUnitPlatform()
}

// (7)
tasks.named('asciidoctor') {
	configurations "asciidoctorExtensions"
	inputs.dir snippetsDir
	dependsOn test
}

// (8)
task copyDocument(type: Copy) {
	dependsOn asciidoctor            // (8-1)
	from file("${asciidoctor.outputDir}")   // (8-2)
	into file("src/main/resources/static/docs")   // (8-3)
}

build {
	dependsOn copyDocument  // (9)
}

// (10)
bootJar {
	dependsOn copyDocument    // (10-1)
	from ("${asciidoctor.outputDir}") {  // (10-2)
		into 'static/docs'     // (10-3)
	}
}
```
<br></br>

### API 문서 스니핏을 사용하기 위한 템플릿(또는 source 파일) 생성
> 마지막으로 할 일은 API 문서 스니핏이 생성 되었을 때 이 스니핏을 사용해서 최종 API 문서로 만들어 주는 템플릿 문서(index.adoc)를 생성하는 것
 - Gradle 기반 프로젝트에서는 아래 경로에 해당하는 디렉토리를 생성해 주어야한다. 
   -  `src/docs/asciidoc/`
 - 비어있는 템플릿 문서(index.adoc)를 생성
   - `src/docs/asciidoc/index.adoc`

> 여기까지 마쳐야 Spring Rest Docs를 사용할 수가 있다.

