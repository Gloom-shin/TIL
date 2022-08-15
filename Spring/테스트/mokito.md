# Mockito? 내가 잘쓰고 있는걸까?
> 스프링으로 테스트케이스를 만들려면 항상 사용하는게 @Mock인데, 굳이 @Mock을 사용하는 이유와
> @Mock을 가지고 있는 mockito 프레임워크에 대해 알아보고자한다.

- 사이드 프로젝트를 진행하면서, Mockito프레임워크를 가지고 테스트를 하려는데, 애매하게 얕게만 알고 있다보니, 응용력이 떨어진다는 것을 느꼈다. 
- 그래서 이번 기회에 내용을 숙지하고자 정리해보았다.


# Mockito
## 굳이 mock를 왜 사용하는 것일까?
- 간단하게 말하자면, Mockito는 단위 테스트를 위한 mocking 프레임워크이다.
- 생성한 Mock 객체의 특정 메소드에 대한 입력-결과값을 미리 지정하거나, 동작 여부를 검증할 수 있다.
- 즉, 테스트케이스가 직접 DB에 접근하여 데이터를 남기게 될수 있으므로, 실제 서비스에 영향을 줄 수도 있고, 
    - 테스트용 인메모리 DB를 활용하더라도 테스트 케이스 내에서도,여러개의 객체를 생성해 DB에 저장하게되는 경우 각 Unit테스트에도 서로 영향을 줄 수 있다. 
    - 아래 코드는 DB저장하고 조회해보는 유닛테스트이다. 각자 테스트케이스를 실행시키면 성공하지만, 함께 돌릴 경우 에러가 난다.(DI에 의한 의존성 영향)
- 이런 Mock 객체를 직접 만들고 관리하기란 쉽지 않은데, Mockito는 이를 편하게 사용하도록 지원해주는 대표적인 테스트 프레임워크이다.
```java
@DataJpaTest
class ReviewBoardRepositoryTest {

    @Autowired
    public ReviewBoardRepository reviewBoardRepository;

    @Test
    @DisplayName("ReviewBoard DB 저장 테스트")
    void saveReview() {
        //given
        ReviewBoard reviewBoard = new ReviewBoard(1L, "서울시", "관악산", "학바위능선", "리뷰제목");
        // 내용과 사진은 생략

        //when
        ReviewBoard saveBoard = reviewBoardRepository.save(reviewBoard);

        //then
        Assertions.assertThat(saveBoard.getId()).isEqualTo(reviewBoard.getId());

    }

    @Test
    @DisplayName("저장된 ReviewBoard 조회Test")
    void findReview() {
        //given
        ReviewBoard saveBoard1 = reviewBoardRepository.save(new ReviewBoard(1L, "서울시", "관악산", "학바위능선", "리뷰제목1"));
        ReviewBoard saveBoard2 = reviewBoardRepository.save(new ReviewBoard(2L, "서울시", "관악산", "학바위능선", "리뷰제목2"));
        //when

        ReviewBoard findBoard1 = reviewBoardRepository.findById(saveBoard1.getId())
                .orElseThrow(() -> new IllegalArgumentException("Board ID : " + saveBoard1.getId() + " 조회할수 없습니다."));
        ReviewBoard findBoard2 = reviewBoardRepository.findById(saveBoard2.getId())
                .orElseThrow(() -> new IllegalArgumentException("Board ID : " + saveBoard2.getId() + " 조회할수 없습니다."));

        //then
        Assertions.assertThat(saveBoard1.getId()).isEqualTo(findBoard1.getId());
        Assertions.assertThat(saveBoard2.getId()).isEqualTo(findBoard2.getId());

    }
}
```
<img src="https://user-images.githubusercontent.com/104331549/184570555-3ccff3cd-5951-41db-a7f3-a94d689cc455.png">

- 이처럼 의존성을 간소화시키고, 테스트 실행속도를 향상시킬 수 있다.

## Mockito의 애노테이션
- `@Mock` 
  - mock 객체를 만들어 반환
  - 실제 인스턴스 없이 가상의 mock 인스턴스를 직접 만들어 사용한다.
- `@Spy`
  - spy 객체를 만들어 반환
  - 실제 인스턴스를 사용해서 mocking함
  - spy 객체는 행위를 지정하지 않으면 객체를 만들 때 사용한 실제 인스턴스의 메서드를 호출한다.
    - 즉, 실제 서비스에 접근하게된다는 것이다.하지만, mock으로써, 행위를 지정시켜줄수 있다.
- `@InjectMocks`
  - `@Mock`이나 `@Spy`가 붙어있는 객체를 `@InjectMocks`가 붙은 객체에 주입시킬 수 있다.
  - 물론 주입도 해당 `@InjectMocks`붙은 타입안에 맴버 클래스와 일치해야 주입 가능하다. 
- `@MockBean` 
  - ApplicationContext에 mock객체를 추가
- `@SpyBean`
  - ApplicationContext에 spy객체를 추가
  
> Mockito의 주요 애노테이션은 알아봤다. mock은 비어있는 것이기 때문에 사용 방법은
> Mock의 입력-결과값을 미리 지정해줘야 한다. 이 방법은 2가지로 가린다.



## Mockito와 BDDMockito
- 테스트 코스를 작성하다 보면,`mock 객체`를
  - 어떤 부분에서는 `when().thenReturn()` 을 통해 사용하고,
  - 어떤 부분에서는 `given().willReturn()` 을 통해 사용한다. 
- import 문을 확인해보면 전자는 `org.mockito.Mockito` 를 후자는 `org.mockito.BDDMockito` 를 import한다.
> 이둘은 무슨 차이 일까?

## BDD? 
 - `Behavior-Driven Development`의 약자로 행위 주도 개발을 말한다.
 - 테스트 대상의 **상태의 변화를 테스트**하는 것이고, **시나리오**를 기반으로 테스트하는 패턴을 권장한다.
 - 그래서 권장하는 기본 패턴은 **Given, When, Then** 구조를 가진다.
### 기존 Mock 테스트 방식

```java
@ExtendWith(MockitoExtension.class)
class MockitoPracticeTest {
    @Mock
    private Skills skills;

    private Person person;

    @BeforeEach
    void setUp() {
        person = new Person("GLOOM", 2, skills);
    }

    @Test
    void hasSkill_AlwaysTrue() {
        when(skills.hasSkill()).thenReturn(true);

        assertThat(person.hasSkill()).isTrue();
    }
}
```
### BDD 테스트 방식
   - 아래와 같이 given 단계에서` when().thenReturn()` 메소드를 사용하게 된다. 
   - given영역에 `when`이 있으니, 헤깔릴 수 있어 이걸 해결하기 위해 나온것이 **`BBDMockito`프레임워크** 이다.
```java
@Test
void hasSkill_AlwaysTrue() {
    // given
    when(skills.hasSkill()).thenReturn(true);

    // when
    boolean actual = person.hasSkill();

    // then
    assertThat(actual).isTrue();
}
```
### BDDMockito 프레임워크 적용
 - BDD 기본 패턴의 given에 해당하는 위치에 이전에 사용한 Mockito의 when() 메서드가 아닌 **given() 메서드가 사용됨**을 알 수 있다.
 - 추가적으로 BDD 기본 패턴의 then에서 사용되는 Mockito에서 제공하는 verify() 도 then().should() 로 대체될 수 있다.
 - 내가 **테스트하고자하는 패턴에 맞게 코드가 읽힐 수 있도록** BDDMockito 프레임워크가 도와준다 
   -  가독성 증가!
```java
@Test
void hasSkill_AlwaysTrue() {
    // given
    given(skills.hasSkill()).willReturn(true);

    // when
    boolean actual = person.hasSkill();

    // then
    assertThat(actual).isTrue();
}
```

### 참고링크
 - [테스트 더블](https://velog.io/@lxxjn0/Test-Double%EC%9D%84-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
 - [Mock, Mockito, BDD](https://dongdd.tistory.com/165)