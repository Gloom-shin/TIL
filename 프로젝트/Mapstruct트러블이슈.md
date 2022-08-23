

# 트러블 이슈
> Mapstruct가 DTO 객체에서 Entity 객체로 매핑을 못해준다
> 여기서 더 문제는 항상 인식을 못하는 것이 아니라, 10번에 1번꼴로 제대로 인식이 된다는 것이다.
---
## 현재 상황
<img src="https://user-images.githubusercontent.com/104331549/185874398-f990105a-8e3e-41f6-b6aa-ff49b678e7b9.png">
 
 - 보시다시피, DTO 객체에서 Entity로 매핑하는 작업을 MapStruct로 작업하였다.
 - 위와 같이 구현을 하기 위해선 총 3개의 파일이 필요하다. 
   1. Entity 객체 
   2. DTO 객체
   3. Mapper 클래스(Controller 클래스는 생략한다.) 
<p align="center"><img src = "https://user-images.githubusercontent.com/104331549/185876638-d102dac9-8945-426b-9657-6163b3763dd9.png" width="60%"> </p>

### build.gradle의 dependencies
```java
dependencies{
    // ..생략
    implementation'org.mapstruct:mapstruct:1.5.2.Final' // Mapstruct
    annotationProcessor'org.mapstruct:mapstruct-processor:1.5.2.Final'


    compileOnly'org.projectlombok:lombok' // 롬복
    annotationProcessor'org.projectlombok:lombok'
    // ..생략
}
```

### Entity 객체 
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class ReviewBoard {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    public Long reviewBoardId;

    @Column(length = 20, nullable = false)// 추후 enum으로 표현
    public String localName;

    @Column(length = 100, nullable = false)// 추후 enum으로 표현
    public String mountainName;

    @Column(length = 100, nullable = false) // 추후 enum으로 표현
    public String mountainCourseName;

    @Column(length = 100, nullable = false)
    public String title;

    @Column(length = 100)
    public String body;  //CLob
    @Column(length = 100)
    public String photo; //BLob

    // 작성 시간 추가필요
}
```

### DTO
```java
public class ReviewBoardDto {

    @Getter
    @AllArgsConstructor(access = AccessLevel.PRIVATE)
    @NoArgsConstructor
    @Builder
    public static class Post {
        @NotBlank
        public String localName;
        @NotBlank
        public String mountainName;
        @NotBlank
        private String mountainCourseName;
        @NotBlank(message = "제목은 공백이 아니어야 합니다.")
        private String title;

        @NotBlank
        private String body;

        private String photo;
    }
}
```
### Mapper
```java
@Mapper(componentModel = "spring")
public interface ReviewBoardMapper {
   ReviewBoard reviewBoardPostToReviewBoard(ReviewBoardDto.Post requestBody);
}
```

<br></br>

## 발생한 문제 
 - 원래대로라면, DTO의 속성값들이 Entity속성값으로 매핑이 되야 하는데, 2개의 값만 매핑되어 있고, 그마저도, getter,setter가 아닌 직접 접근하여 값을 대입해주고 있는 것을 볼 수가 있다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185897986-3a133ecb-4b5f-448e-8aa3-452f7acd6051.png" width="65%"></p>

 - 혹여나, DTO에 값이 전부 들어오지 않는 경우를 대비해서, `Entity`에 `@Builder`애노테이션을 활용하여 생성자를 만들어도 아래와 같이, 매핑은 제대로 되질않고 null값으로 채운다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/186064184-86b9d05f-c991-4e95-a904-88cc054abeda.png" width="65%"></p>

>  만약 해결방법을 원하신다면, 시도해본방법은 건너뛰시면 된다.

<br></br>
<br></br>
---
## 시도해본 방법
### 직접 매핑해주기
 - `@Mapping()`을 활용하여, source와 target을 정하면, DTO의 속성값과 Entity의 속성값을 연결 시켜줄 수가 있다. 
 <p align="center"><img src="https://user-images.githubusercontent.com/104331549/186069889-6958c48d-8fc9-4918-97e5-e8384b1edc2e.png" width="75%"></p>

```java
@Mapper(componentModel = "spring")
public interface ReviewBoardMapper {
    @Mapping(source = "mountainCourseName", target = "mountainCourseName") // 추가된 부분
    ReviewBoard reviewBoardPostToReviewBoard(ReviewBoardDto.Post requestBody);

}
```
#### 결과 
 - 이렇게 하면 가끔씩, `MapperImpe` 구현체가 제대로 매핑되어 나올때가 있지만, 모든걸 다시 세팅하게되면 
 - `java: No property named "mountainCourseName" exists in source parameter(s). Did you mean "mountainName"?` 속성값을 찾지 못하는 에러가 발생한다. 
 - 즉, 안정하지 못하는 상태라는 것이다. 

<br></br>

### 변수명 변경 
 - 혹시나, 매핑과정에서 잘못된게 아닌가 싶어, Entity와 DTo의 변수명을 바꿔보았다.
   - `mountainCourseName` - > `courseName`

#### Entity 
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class ReviewBoard {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    public Long reviewBoardId;

    @Column(length = 20, nullable = false)
    public String localName;

    @Column(length = 100, nullable = false)
    public String mountainName;

    @Column(length = 100, nullable = false) 
    public String courseName;  // 변수명 변경

    @Column(length = 100, nullable = false)
    public String title;

    @Column(length = 100)
    public String body;  
    @Column(length = 100)
    public String photo; 
}
```

#### DTO
```java
public class ReviewBoardDto {

    @Getter
    @AllArgsConstructor(access = AccessLevel.PRIVATE)
    @NoArgsConstructor
    @Builder
    public static class Post {
        @NotBlank
        public String localName;
        @NotBlank
        public String mountainName;
        @NotBlank
        private String courseName; //변수명 변경
        @NotBlank(message = "제목은 공백이 아니어야 합니다.")
        private String title;

        @NotBlank
        private String body;

        private String photo;
    }
}
```

### 라이브러리 버전 다운
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185878083-ae2dd2fe-3642-4f13-8ed9-12e7d5a3d06f.png" width="65%"></p>


### 버전다운과 변수명 변경 결과 
> 하지만 결과는 똑같았다. 

<br></br>
<br></br>

### Gradle dependencies 순서 바꾸기

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/186077485-551bc9ed-d47d-4a23-864d-f019e4903de7.png" width="65%"></p>

- `MapStruct`의 `MapperImpl` 은 Entity의 `setter` 와 DTO의 `getter`로 생성이 된다.
- 즉,  `@Lombok`의 영향을 받을 수 밖에 없는데, `Gradle dependencies` 순서에 따라 생성되는 코드가 다르게 출력된다고 하여, 순서를 바꾸어 보았다.
- `mapstruct` 가 `lombok` 뒤에 오는 경우, Entity(target)객체에 `@Builder`가 있어도 무시하고 `생성자 + setter` 를 사용한다고 한다.
- [참고한 래퍼런스](https://wise-develop.tistory.com/18)

#### 결과
 - 반대로 말하면, 기존 코드는 `@Builder`를 먼저 인식하고, `MapStruct`가 `MapperImpe(구현체)`를 생성했다는 것이다.
   - 좀 더 부연 설명하자면, Entity의 Property값과 생성자가 모두 생성되고 나서 Setter가 생성되는데, 생성자에 해당하는 @Builder가 먼저 생성되고, 그과정에서 MapStruct가 접근하게되어, 
   - Setter를 사용하지도 못하고, Null값만 존재하는 필드값이 매핑되어 버리는 것이다. 게다가 빌드과정이 잘 된다면 운이 좋게 한번 성공하게 되는 것도 어느정도 설명이 된다.
 - 순서를 바꿈으로써, `@Builder`를 무시하고, `Constructors`와 `Setter`로만 구현체를 만들었다고 볼 수 있다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/186077314-990bb3ab-151a-4556-a66f-31beb5e8fcf5.png" width="65%"></p>

<br></br>
<br></br>

## 해결방안
> 위 방식처럼 내부적인 로직을 알고 해결할 수 있지만,상황에 따라 에러가 발생하기에 안정적이라고는 하기 어렵다. 
> 복잡한 과정을 고려하지않아도 한번에 해결할 수 있는 방법이 있다.
### 라이브러리 추가 
- 참고자료 : [mapstruct 공식문서](https://mapstruct.org/documentation/stable/reference/html/#lombok)
- 공식문서에 따르면 아래와 같다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185892733-48d4600a-4cb9-4fa6-89f4-649522678f7a.png" width="65%"></p>

  - `MapStruct`는 1.2.0 베타1버전 이후부터 `Lombok1.16.14` 버전과 함계 사용할 수 있습니다.
  - `MapStruct`는 `Getter`, `Setter`, `Constructors` 들을 활용하여 `Mapper 구현체`를 만들어 냅니다.
  - `Lombok 1.18.16`버전부터 내부 로직이 바뀌어, `MapStruct`이 작업할때,  `Lombok`에서 중지되게 됩니다.
  - 그러므로 `annotation processor lombok-mapstruct-binding`를 추가해야 합니다.

```java
dependencies{
    annotationProcessor"org.projectlombok:lombok-mapstruct-binding:0.2.0" // 추가
}
```

