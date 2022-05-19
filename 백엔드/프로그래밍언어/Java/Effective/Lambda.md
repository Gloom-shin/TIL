# 람다식 사용해보기 
기본적으로 자바에서 람다식을 사용하려면, 단 하나의 추상메서드만 포함하는 인터페이스를 사용하면 좋다.   
게다가 이런 인터페이스를 **함수형인터페이스**라고 따로 불리우기도 한다.


## 테스트 

### 매개변수와 리턴값이 없는 람다식
먼저 interface부터 만들어주자.

\[MyFunctionalInterface.interface\]
```
@FunctionalInterface // 해줘도되고 안해줘도 된다.
public interface MyFunctionalInterface {
    public void lambda();
}
```
- 애너테이션때 나온 `@FunctionalInterface`은 해줘도되고 안해줘도 되지만, 안정성을 위해 붙여주는게 좋다.
- 그럼 이제 `class`에서 이 함수형 인터페이스를 불러와 람다식을 사용해보자

<br></br>

\[MyFunctionalInterfaceExample.java\]
- class명과 main 메소드는 편의상 생략

\[방법1\]
```
MyFunctionalInterface example;  // 함수형 인터페이스 선언

example = () -> {
    String str = "Calling Method, First";   //추상메소드이기에, 내부를 구현해준다.
    System.out.println(str);
};

example.lambda(); // 실행 Calling Method, First
```
- 람다식이 잘 동작한다.
- 만약 실행문이 한줄로 가능하다면 `{}` 생략도 가능하다.

\[방법2\]
```
example = () -> System.out.println("Calling method, Second");
example.lambda(); // Calling method, Second
```

### 매개변수가 있는 람다식
이번엔 리턴값은 없지만, 매개변수가 있는 람다식을 구현해보자
먼저 함수형 interface를 만들어준다.

\[함수형 인터페이스 생성\]
```
public interface MyFunctionalInterface {
    public void lambda(int a);
}
```
- 위 추상메소드는 인자를 `int`형으로 하나만 갖는다.

<br></br>

\[람다식 구현\]
```
MyFunctionalInterface example;

// 첫번째 방법
example = (x) -> {
    int result = x*5;
    System.out.println(result);
};
example.lambda(2);  // 10 출력

// 두번째 방법
example = (x) -> System.out.println(x*3);
example.lambda(2);  // 6출력
```
 - 함수형 인터페이스에서 `lambda(int a)`로 매개변수를 하나로 지정해줬기 때문에, 람다식을 구현할 때도 매개변수를 하나 넣어줘야한다.


### 리턴값이 있는 람다식
이번엔 매개변수도 있고, 리턴값도 가지는 람다식을 구현해보자

\[함수형 인터페이스 생성\]
```
public interface MyFunctionalInterface {
    public int lambda(int x, int y);
}
```

- 이번엔 매개변수를 2개 넣어줘서, 람다식에도 똑같이 적용해야한다.
- 그리고 이번에 구현할 수 있는 방식은 생각 보다 많다.
\[람다식 구현\]
```
MyFunctionalInterface example;

// 방법1
example = (x, y) -> {
    int add = x + y;
    return add;
};
int result = example.lambda(2, 5);
System.out.println(result);              // 7


// 방법2
example = (x, y) -> x + y;         //return 생략가능
int result2 = example.lambda(3, 1);
System.out.println(result2);       // 4

// 방법3
example = (x, y) -> sum(x,y);      // import  java.lang.Integer.sum
int result3 = example.lambda(4, 8);
System.out.println(result3);       // 12

// 방법4
example = Integer::sum;
int result4 = example.lambda(6, 2);
System.out.println(result4);       // 8
```
