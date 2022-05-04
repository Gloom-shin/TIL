# String 
- 먼저 문자열 객체인 String에 대해서 짚고 넘어가자.

## String_문자열이란?
- java에서 String이란 매우 비중있는 데이터타입이다. 
- 하지만, int, double과는 다르게 reference 타입. 즉, 객체인 데이터 타입이다. 

### String 과 char[]의 차이
  - 물론 String이 "문자를 연이어 늘어놓은것"이 라는 의미가 있어서 char[]과 같은 뜻이로 쓰이지만, 
  - String과 char[] 은 명확한 차이가 있다. 
     - char[] 내부의 값을 읽는 것을 물론이고, 수정을 할수 있다. 
     - 하지만 String은 그저 읽을 수만 있을 뿐이다. 
     - 이것이 이유는 String의 가장 큰 특징인 __불변성(Immutable)__ 을 가지고 있기 때문!

### 불변성(Immutable) 그게 뭔데? 
  - 할당된 공간이 변하지 않는 특성을 말한다. 
  - 예를 들어 아래와 같이 문자열`str`에 더하기 연산으로 "Test"라는 문자열을 더하면, `str = "문자열Test"`가 된다.
  ```
  String str = "문자열";
  str = str + "Test";
  System.out.println(str); // 문자열Test
  ```
  - 하지만 str의 주소를 출력해보면 원래 str에서 값을 더한것인지, 새로운 값으로 덮어씌운것인지를 알 수 있다.
  ```
  String str = "문자열";
  System.out.println(str.hashcode());   // 47693212
  
  str = str + "Test";
  System.out.println(str.hashcode());   //795822158
  ```
