# Optional 클래스 
- Java8에 추가된 새로운 API이다. 
- `Integer`나 `Double` 클래스 처럼 `T`타입의 객체를 포장해주는 `래퍼 클래스` 이다. 
- 따라서, Optional 인스턴스는 모든 타입의 참조 변수를 저장할 수 있다. 

 > java에 대표적인 에러가 `NullPointerException`인데 이 고통스러운 null 처리를 할 수있게 도와주는 클래스라고 보면된다.

<br></br>
## NullPointerException

### 기존의 null 처리
 - 기존의 자바코드는 (`String`이나 `int`같은)변수에 `null`값이 들어오면, `NullPointerException`이 발생하고 프로그램이 종료되었다. 
 - 그래서 이 에러를 예측하고, 별도의 처리를 해줘야 됬다.
   - `if`문으로 null 일시 값을 초기화 해주거나, 
   - `try ~ catch`문으로 처리
```java
// try catch 문을 이용한 null 처리
try {
	System.out.println(test.getStr());
} catch (NullPointerException e) {
	System.out.println("str is null");
}
```
 - 하지만, 이 방식은 핵심 로직과 관련된 코드의 **가독성을 떨어뜨리게** 된다.

### Optional<> 의 null 처리 
- `Optional<T>`로 변수를 optional로 씌워주게되면, 해당 값이 null 이여도 에러가 발생하지 않는다. 
  - `null`이 아니면 명시된 값을 가지는 Optional 객체를 반환하고
  - `null`이면 비어있는 Optional객체를 반환한다.
     
  <br></br>
  <br></br>

## Optional 메소드 
> Optional은 `NullPointerException` 를 피하기위한 몇가지 메소드를 제공한다. 

 - `isPresent()`
   -   Optional 객체에 저장된 값이 null이면 `false`, 저장된 값이 있으면 `true`를 반환한다.
 - `get()`
   - OOptional 객체에 저장된 값에 접근할 수 있다.
 - `orElse(T other)`
   -  저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 값을 반환함.
 - `orElseGet(()->결과)`
   - 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 람다 표현식의 결괏값을 반환함.
 - `orElseThrow(()->에러)`
   - 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 예외를 발생시킴. 

<br></br>
<br></br>

## 사용사례
### CrudRepository 연결 후 반환 값