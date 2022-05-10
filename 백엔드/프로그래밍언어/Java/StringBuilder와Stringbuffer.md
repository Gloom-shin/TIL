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
 - 주소값 자체가 바뀌는 것을 보아, String은 새로운 값으로 덮어씌우기가 됨을 알 수 있다.

> 이말은, 문자열 변경이 자주일어나는 프로그램에서 String을 사용한다면 비효율적일수도 있다는 것!!⭐⭐⭐


---
<br></br>

# StringBuilder 와 StringBuffer👀
 - 위 문제를 해결하기위해서 가변성(mutable)이 있는 이 2가지 클래스를 사용한다.

## AbstractStringBuilder 
- 이름에도 알 수 있듯이 추상 클래스이다. 
- StringBuilder와 StringBuffer 둘다 AbstractStringBuilder 라는 추상 클래스를 상속받아 구현된 클래스이다. 
- 이 추상클래스 내부에는 멤버변수 2가지가 존재한다.


### 맴버변수
 1. value : 문자열의 값을 저장하는 byte형 배열
 2. count : 현재 문자열 크기의 값을 가지는 int형 변수 
 
### append() 메소드 
```
    public AbstractStringBuilder append(String str) {
        if (str == null) {
            return appendNull();
        }
        int len = str.length();
        ensureCapacityInternal(count + len);  //추가되는 문자열 길이만큼 늘리고
        putStringAt(count, str); // 값 추가
        count += len; // 문자열 길이 갱신
        return this;
    }
```
- 내부동작을 보면 값이 변경되더라도 **같은 주소공간을 참조**하게 되는 것이며, **값이 변경되는 가변성**을 띄게 된다.   


## 차이점 ⭐
- 가장 큰 한가지 차이점이 존재하는데, 바로 동기화(Synchronization) 이다.
- StringBuffer는 동기화를 지원하지만, StringBuilder는 동기화를 지원하지 않는다.

### 동기화란?
 - 여러개의 스레드가 한 개의 자원에 접근하려고 할때, 현재 데이터를 사용하고 있는 스레드가 있을 경우, 그 스레드를 제외하고 나머지 스레드들이 접근할 수 없도록 막는 것을 말한다. 
 - 물론 현재 데이터를 사용하는 스레드가 사용을 마치면, 다음 스레드가 접근할 수 있다. 


### 정리 ✔
 - String : 문자열 연산이 적고 멀티스레드 환경일 경우
 - StringBuffer : 문자열 연산이 많고 멀티스레드 환경일 경우
 - StringBuilder : 문자열 연산이 많고 단일스레드 이거나 동기화를 고려하지않아도 되는 경우
 
 
