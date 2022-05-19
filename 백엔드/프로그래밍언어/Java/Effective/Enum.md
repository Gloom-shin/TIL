# Enum(열거형)
- 서로 연관된 상수들의 집합을 의미한다. 
- 예를들어 
    - 계절의 경우  :  {SPRING, SUMMER, FALL, WINTER}
    - 일주일의 경우 : {SUNDAY , MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY}
    - 달력의 경우  :  {JANUARY, FEBRURY, MARCH, APRIL, MAY, JUNE, JULY, AUGUST, SEPTEMBER, OCTOBER, NOVEMBER, DECEMBER}
- 이런 식의 서로 연관된 상수(변하지않는 값)들을 하나로 묶어 관리할 수 있다.
- 참고로 상수는 관례적으로 대문자로 작성한다.


## Enum 클래스

### 사용법
- class 대신에 enum으로 만들어 주고, enum명을 입력한 뒤, 배열처럼 상수를 나열해 주면 된다.   

**\[level.java\]** _(파일명은 java로 나옴에 유의하자)_
```
enum Level {
  LOW,
  MEDIUM,
  HIGH
}
```
- 그리고 class가 있는 곳에서 enum을 할당하여 사용한다. _(new연산을 하지않음에 유의!)_  

#### \[Main.java\]
```
public class Main {
  public static void main(String[] args) {
    Level level = Level.MEDIUM;

    switch(level) {
      case LOW:
        System.out.println("Low level");
        break;
      case MEDIUM:
         System.out.println("Medium level");
        break;
      case HIGH:
        System.out.println("High level");
        break;
    }
  }
}
```

### 메소드 
|Return-Type|Method(paramater)|detail|
|--|--|--|
|String|name()|열거 객체가 가지고 있는 문자열을 리턴하며, 리턴되는 문자열은 열거타입을 정의할 때 사용한 상수 이름과 동일하다.|
|int|ordinal()|열거 객체의 순법(0부터 시작)을 리턴(ex)Seasons.SPRING = 0, Seasons.WINTER = 3|
|int|comparaTo(비교값)|주어진 매개값과 비교해서 **순번 차이**를 리턴한다.|
|열거타입|ValueOf(String name)|주어진 문자열의 **열거 객체 하나**를 리턴한다.|
|열거배열|values()| 모든 열거들을 **배열**로 리턴한다.|


## enum의 상숫값
- 위 `ordinal()`과 `comparaTo()`메소드에서 알 수 있듯이 enum(열거)객체는 각각의 순번값을 가지고 있다. 
- 별도로 지정해주지 않으면, defalut값으로 순서대로 처음이 `0`, n 번째가 `n-1`을 갖는 배열 index와 같은 구조를 띄고있다.
- 하지만, 이러한 순번값 외에 상숫값을 원하는 값으로 별도로 명시해줄 수 있다. 
### 상수값 추가
 - enum(열거)객체 뒤에 `()`하고 안에 상숫값을 넣어주면, 원하는 값을 지정해서 넣을 수 있다.

**\[level.java\]**
```
public enum Level {
    HIGH(5), MEDIUM(1), LOW(-10);
    
    private final int value;

    Level(int value) { this.value = value; }

    public int getValue() { return value; }
}
```
 - 게다가, 위와 같이 enum객체들을 `;`로 구분지어 enum 클래스 안에 **변수와 메소드도 사용**할 수 있다.



#### \[Main.java\]
```
public static void main(String[] args) {
    Level levelH = Level.HIGH;
    Level levelM = Level.MEDIUM;
    Level levelL = Level.LOW;

    System.out.println(levelH.ordinal());   // 0
    System.out.println(levelH.getValue());  // 5
    System.out.println(levelL.compareTo(levelH));  // 2 -0 = 2
}
```

## enum을 사용하는 이유 
`enum`과 같이 비교되는 것이 같은 상수취급을 받는 `public final static`이다.    
하지만, `public final static`의 단점이 있는데, `enum`은 그점을 해결해준다.
- 상수명의 중복을 피할 수 있다. 
- 타입에 대한 안정성을 보장받는다
- 코드가 단순해지고, 가독성이 좋아진다.
- **switch문에 사용할 수 있다.** 
