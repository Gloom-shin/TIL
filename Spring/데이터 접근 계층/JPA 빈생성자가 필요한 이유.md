# JPA사용 
 - JPA를 간단하게 사용하는 방법은 매우 간단하다. 
 - 인터페이스로 `Repository`를 만들고, `extend JpaRepository<(엔티티타입),(@Id타입)>`를 넣어 주면 된다.
 - 구현체를 만들어 주는 `@Repository` 어노테이션을 만들어 주면된다.
 - 그럼 왠만한 CRUD는 사용가능해진다. 

## JPA 테스트시 나타난 오류
> 간단한 JPA Repository를 만들고, 간단한 Save()테스트를 해보는 과정에 아래와 같은 에러가 나타났다.

- `org.springframework.orm.jpa.JpaSystemException: No default constructor for entity`
  - 엔티티 기본 생성자가 없습니다. 라는 에러이다. 

### JPA Entity 
 - 결론부터 말하자면, JPA의 Entity는 반드시 파라미터가 없는 기본 생성자를 지녀야 한다.
 - 그리고 이 기본 생성자는 접근 지정자가 `public`, `protected` 이어야 한다. 

### 기본 생성자가 필요한 이유
 - Spring Data JPA 에서 Entity에 기본 생성자가 필요한 이유는 
   - JPA의 하이버네이트는 내부적으로 Class.newInstance()라는 리플렉션을 이용해 해당 Entity의 기본 생성자를 호출해서 객체를 생성한다.
   - 그리고 그 객체를 동적으로 필드값들을 넣어 주기위해 **Reflection API를 활용하기 때문**이다.
 - JPA는 DB값을 객체 필드에 주입할 때,
   - 기본 생성자로 객체를 생성한 후, 
   - Reflection API를 사용하여 값을 매핑하기 때문이다.
 - 즉, 기본 생성자가 없다면 Reflection은 해당 객체를 생성할 수 없기때문에, 기본생성자가 꼭 필요하다.

<br></br>


## Reflection API
 - 구체적인 클래스 타입을 알지 못해도 그 클래스의 정보(메서드, 타입, 변수등)에 접근 할 수 있게 해주는 자바 API이다.
   - [참고 자료](https://tecoble.techcourse.co.kr/post/2020-07-16-reflection-api/)
 - 자바에서는 JVM이 실행되면 작성된 자바 코드가 static 영역에 저장된다.
   - Reflection API는 이 정보를 활용한다. 
   - 그래서 클래스 이름만 알고 있다면 언제든 static 영역을 뒤져서 정보(변수, 메서드등)를 가져올 수 있는 것이다. 
   - 다만 Reflection API는 생성자의 **인자정보는 가져올 수가 없다**. 
     - 기본생성자만 됨

<br></br>

## JPA의 지연로딩과 즉시로딩
 - JPA는 매핑한 Entity를 조회할 때 두 가지 전략을 사용한다.
   - 조회 시점에 함께 가져오는 `EAGER`
   - 매핑한 Entity를 사용할 때 조회하는 `LAZY`
     - 이때 지연로딩은 임시로 하이버네이트가 생성한 Proxy객체를 생성하고 가르킨다.

### Proxy 객체로 인한 접근제어자
 - proxy 객체는 직접 만든 class 를 상속하기 때문에 public, protected 기본 생성자를 필요로 하게 된다.
 - 결국 만약 public, protected 생성자가 없다면 proxy 객체를 사용할 수 없게 되는 것이다.


### 느낀점
 - 기본생성자가 필요한 이유를 어린짐작으로 알게되었다.
 - 하지만,전체적인 구조만 알게되었을뿐, 구체적인 원리와 내부적으로 어떻게 돌아가는지는 잘 알지못한다. 
   - 파고파도 더 deep한 내용이 나올것 같아, 현재로썬 하던일에 집중하기로 했다.
 - 또한, 지연로딩과 즉시로딩을 관련해서 직접 사용하거나, 실습하게될 일이 생기면 구체적인 예시를 만들어 내용을 보충해도 좋을 것 같다.