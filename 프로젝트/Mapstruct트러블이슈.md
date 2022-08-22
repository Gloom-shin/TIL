# Mapper란? 

# Mapperstruct란?  



# 발생한 문제
> Mapstruct가 DTO 객체에서 Entity 객체로 매핑을 못해준다
> 여기서 더 문제는 항상 인식을 못하는 것이 아니라, 10번에 1번꼴로 제대로 인식이 된다는 것이다.

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

> 만약 해결방법을 원하신다면, 시도해본방법은 건너뛰시면 된다.

## 시도해본 방법

### 라이브러리 버전 다운 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185878083-ae2dd2fe-3642-4f13-8ed9-12e7d5a3d06f.png" width="65%"></p>

- `MapStruct`의 `MapperImpl` 은 `setter`(`@Lombok`으로 처리 가능)로 생성이 된다.

- [참고링크](https://ryanwoo.tistory.com/240)

### 변수명 변경 


### Setter 만들기
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/185878083-ae2dd2fe-3642-4f13-8ed9-12e7d5a3d06f.png" width="65%"></p>


## 해결방안
### 라이브러리 추가 
- 참고자료 : [mapstruct 공식문서](https://mapstruct.org/documentation/stable/reference/html/#lombok)
- 공식문서에 따르면 아래와 같다.


```java
        dependencies{
        annotationProcessor"org.projectlombok:lombok-mapstruct-binding:0.2.0" // 추가
        }
```



