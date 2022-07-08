# JPA
> 애플리케이션에서 데이터 접근 계층을 거쳐, 데이터베이스에 도달하는 과정이 중요하여   
> JPA 플로우에대해 다시 언급하고자 한다. 


## JPA Flow

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177975187-7880540d-4124-4703-ae31-2ddc1eeeb741.png" width="80%"></p>

- 위 그림과 같이, Application은 JPA와 Hibernate를 거쳐 JDBC 지나, DB데이블에 쿼리문으로 접근한다. 
- 여기서 중요한 것은 이 덕분에 DB를 생성하고, 관리하고, 심지어 테이블 간의 연관관계(외래키)까지 Application에서 지정해줄 수 있다는 것이다.
    - 물론 JDBC로도 위 기능이 가능하지만, 자바외의 sql문등이 필요로 하다. 

> 결국 자바로만 DB를 관리하려면 JPA를 사용해야하는 것이다.

<br></br>
<br></br>

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177976124-70fbb52b-b0f1-4213-b0f9-1c2c84dc2dd9.png" width="80%"></p>

 - 여기가 매우 중요한데, DB테이블 개념이 아닌, java의 `객체 파일`을 기준으로 엔티티가 만들어 진다는 것이다

> 그럼 하나의 예시를 들어보자

<br></br>
<br></br>

### 예시 
 - Test1.java
```java 
 @Entity // 그외 필요한 게터,세터 생략
 class Test1 {
    long test1Id = 5;
    String name ="test";
 }
```

- Test2.java
```java 
 @Entity // 그외 필요한 게터,세터 생략
 class Test2 {
    long test2Id = 5;
    long testId = // Test1의 Id 값을 넣고자한다.
 }
```

 - 위처럼, java로 만든 엔티티 클래스가 2개 있다고 가정하자
 - 하나는 외부에서 추가 입력을 하면 추가되는 Test1클래스(엔티티)이고, 
 - 다른 하나는 Test1의 `id`를 외래키로 갖는 Test2클래스(엔티티) 이다. 
 - 위처럼, Test2.class에서, 필드 test1Id 값을 가져올라면, `Test.class`를 불러오고, `getTestId()`메소드로
값을 불러와야 한다.
   - `Test1.getTestId()`
> 허나, java 클래스는 엔티티.   
> 즉, DB테이블의 Column을 나타내는 필드만 들어가고,   
> 실제 행이 추가되는 경우는 클래스파일을 사용하는게 아닌, 인스턴스(행 추가)를 만들어 줘야한다. 

<br></br>
<br></br>


<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177978080-1e6258f4-1e70-40d1-85f4-9cf36f0d7297.png" width="80%"></p>

- 이제부터 간단하게 `Test1`클래스는 1번클래스라 칭하고, `Test2`클래스는 2번 클래스라고 칭하겠다.
- 1번 클래스에서 행이 추가될때 마다, 인스턴스를 만들어 준다. 
- 1번 클래스의 인스턴스가 어떤 행위를 할때마다, 외래키를 가진 2번 클래스도 인스턴스를 생성해준다. 
- 즉, 인스턴스와 인스턴스 끼리 연결이 되야한다. 
  - 첫번째 생성된 1번 인스턴스와 첫번째 생성된 2번 인스턴스 
  - 두번째 생성된 1번 인스턴스와 두번째 생성된 2번 인스턴스 
  - 즉, `일대일 매핑`하는 경우이다.

> 하지만, 실제 인스턴스끼리 연결해주는 방법이 없다.   
> getter()메소드도 불가능 하다.

<br></br>
<br></br>

### 내가 매핑을 못했던 이유 
 - 테이블관계로만 생각했었기에, 변수와 변수만 연결하면 된다고만 생각했지, `class`타입과 `class`타입으로 연결시키는 것은 이해가지 않았었다.


<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177980354-50ee390a-6161-47d2-95ec-061e846c2154.png" width=""></p>

 - 하지만, 인스턴스와 인스턴스는 지역변수로 연결시키는 것은 매우 어렵게 느껴졌기에, `class`타입으로 컬럼을 만들어 연결시키는 것이 이해가 되었다.
 - 게다가 원하는 정보가 해당 클래스의 변수하나이기에, 변수를 찾아야하지만, 
 - 먼저 인스턴스와 인스턴스끼리 연결을 시켜야했기에, 합치는 과정을 먼저하고, 
 - 필요한 변수는 이미 테이블로 변환이 된 상태이기에, 테이블 컬럼명으로 지정하여 가져올 수 있는 것!
 - 반대로, 매핑관계에서 주인이 아닌쪽은, `mappedBy`로 양방향 연결을 할 수 있으며, 해당 변수명과 일치하면 클래스에서 찾아 매핑한다.

<br></br>

### 일대다 관계

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177980221-f9e69cc4-dfbf-4bde-be35-b56804bc0365.png" width="80%"></p>

> 자연스럽게 일대다 관계도 이해가 되었다.   





