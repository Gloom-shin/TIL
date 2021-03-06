# 백엔드 자바 언어

<br></br>

## 자바의 특징!💪 

### 1.운영체제에 독립적☑
  - 자바 이전에는 특정 CPU에 따라 작동하거나, 특정 OS에 따라 다르게 작성해야하는 프로그래밍 언어가 대부분이었다. 
  - 이 문제를 해결하고자 나온것이 JVM(자바가상머신)이다.(모든 운영체제에서 실행이 가능하다.)
  
### 2. 객체 지향 언어(Object Oriented Programming,OOP)🔄
  - 객체 지향 언어의 대표적인 특징은 5가지가 있는데, **상속 , 캡슐화,정보은닉, 다형성, 추상화** 가 있다. 
      1. 상속 : 부모클래스의 변수와 메서드를 자식 클래스가 전부 물려받는 것
      2. 캡슐화 : 객체의 변수 및 메서드를 외부 객체가 함부로 건드리지 못하게 감싸는 개념
      3. 정보은닉 : 캡슐화에서 가장 중요한 개념, 다른 객체에 자신의 정보를 숨기고 자신의 연산만을 통하여 접근을 허용하는 것
         - 클래스의 getter/setter등을 통해 은닉한다.
      4. 다형성 : "다양한" + "변형"의 합성어 이다.  즉, 하나의 객체가 여러가지 타입을 가질 수 있는 것을 의미한다..
      5. 추상화 : 자바에서 공통의 속성,기능을 묶어 이름을 붙이는 것 
          - 추상클래스, 인터페이스로 구현
          

### 3. 함수형 프로그래밍 지원🚩
 - 함수형 프로그래밍을 지원하는 문법인 람다식과 스트림이 추가되었다.
 
### 4. 자동메모리 관리 (Garbage Collection)🗑
  - 자바는 자동으로 메모리를 관리해주는 기능을 추가 했다.
  - 이전의 언어들은 메모리의 생성과 소멸을 개발자가 직접 설계해야 했다.
      - 자바는 가비지 컬렉터(Garbage Collector)를 실행시켜 자동으로 사용하지 않는 메모리를 수거한다.
      - 물론 가비지 컬렉터(Garbage Collector)를 직접 호출하여 메모리를 해제할 수 도 있다.

### 5. 동적 로딩을 지원🚚
  - 자바는 애플리케이션이 실행될 때 모든 객체가 생성되지않고, 객체가 필요한 시점에 클래스를 동적 로딩하여 생성한다.
  - 동적 로딩의 장점은 클래스 일부가 변경되어도 다시 컴파일 하지않아도 되는 이점이 있다.
  - 이 덕에 비교적 적은 작업으로 처리할 수 있는 유연성도 가진다.
  - 다만 단점은 그때그때 메모리에서 불러오기에 프로그램 실행 속도가 느린편이다.->(static을 붙이면 바로 메모리에 적재된다.)

### 6. 멀티 쓰레드 프로그래밍 가능🔱
  - 하나의 프로그램에서 여러개의 쓰레드가 동시에 실행할 수 있는 환경을 지원한다. 
  - 다른 언어는 운영체제의 도움을 받아 멀티쓰레드를 수행하지만, 자바는 운영체제 지원 없이도 멀티 스레드 프로그래밍이 가능하다.
      - 예를 들어, 음악프로그램에서 쓰레드 하나는 파일을 다운로드하고, 다른 쓰레드 하나는 스트리밍을 할 수 있다.
      

