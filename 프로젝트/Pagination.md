# Pagination
 - 조회기능하면 빠질수 없는 것이 `Pagination`이다. 
### 페이지네이션(Pagination)이란?
 - 조회할 데이터가 많지않다면 상관이 없지만, 조회하고자 하는 데이터가 1만건이 있는데, 한번에 가져오게되면 매우 불편한 상황이 벌어진다.
 - 그래서  데이터를 모두 요청하는 것이 아니라 한 페이지에 일정 개수 만큼만 나누어서 달라고 요청하는 것을 페이지네이션(Pagination)이라 한다.
 - 보통 페이지네이션의 경우 클라이언트에서 HTTP요청으로 `QueryParameter`에 포함하여 요청하는게 대부분이다.


## Pageable JPA에서 사용하기
 - 기본적으로 JPA Repository로 상속하여 사용하고 있는 `JpaRepository`인터페이스는 
   `PagingAndSortingRepository` 과 `QueryByExampleExecutor<T>`을 상속하고 있다. 
   - 여기서 중요한 것은 `PagingAndSortingRepository`이다.  
     <img src="https://user-images.githubusercontent.com/104331549/185543971-81fb9484-3d4c-4885-924c-d3e6b382b6c0.png">

   - 위 이미지에서 알 수 있듯이, Sort관련된 변수와 Pageable 관련된 변수를 입력할 수 있다.
     - 즉, 정렬별로 조회할 수 있고, pageable 별로 조회할 수 있다는 것이다.

### Pageable
- 추후 내용 추가할 예정
- [참고링크 Pageable을 이용한 Pagination을 처리하는 다양한 방법](https://tecoble.techcourse.co.kr/post/2021-08-15-pageable/)



## JPA - Repository 에러 
 - JPA를 사용하여 Repository에 메소드 개발 중에 나타난 에러는 `No property  xxx found for type xxxx` 이다.
   - 여기서 나타나는 xxx는 대부분 Repository 메소드명의 `By` 다음을 나타내며, 뒤의 xxx는 엔티티를 나타낸다.
 - 아래 코드를 예로 들어보자
   - 내가 원하는 값으로 페이지네이션 조회기능을 사용하고자 `findByName`를 추가 시켰다.
   - 하지만, `No property 'name' found for type 'Member'` 에러가 뜬다면 이 것은 Member Entity를 확인해봐야한다.
### Member Repository
```java
public interface Repository extends JpaRepository<Member,Long> {
    Page<Member> findByName(String Name, PageRequest pageRequest);
}
```
### Member 엔티티
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public class Member{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    public Long memberdId;

    @Column(length = 20 ,nullable = false)// 추후 enum으로 표현
    public String localName;
    
}
```
 - 보시다시피 Member Entity에는 속성값으로 `memberdId`, `localName` 밖에 없다. 
 - 그런데 Repository 에서는 `findByName`으로 `Name` 속성을 찾고 있으니, 위와 같은 에러가 발생한 것이다.

### 해결방안 
 - 방법은 간단하다. 속성값을 맞춰주면 된다. 
 - Repository 에서는 `findByName`를 `findByLocalName` 으로 바꿔주면 해결된다. 
   - 여기서 Repository는 `LocalName`이고 Member의 Entity에서는 `localName` 인 이유는 JPA, 즉 하이버네이트가 데이터베이스로 변환 시켜주는 과정에서 
   - 컬럼명이 `local_name`으로 모두 소문자로 변경되며, 대문자를 기순으로 `_`(언더바)가 생기기에, 처리할 수 있는 것이다.

