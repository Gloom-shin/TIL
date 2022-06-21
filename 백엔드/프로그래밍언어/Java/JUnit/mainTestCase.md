## 의문점이 생기기 전 상황

웹페이지에서 코딩테스트 문제를 풀다보니, 문법적으로 내가 잘맞추고 있나 확인하기위해,  
인텔리제이와 같은 툴에 도움을 받는 경우가 종종있다.  
하지만, 툴에 똑같이 코드를 옮겨 적는다고, 테스트 케이스를 바로 확인할 수 있는 것이 아니다.   
그래서, 보통은 그자리에서 instance를 만들어 확인했었지만,   
문득 JUnit으로 확인해보면 좋겠다라는 생각에 JUnit를 켜봤다. 

> 그런데, 문제는 여기서 발생했다. 

# Main메소드가 있는 JUuit Test
- 평소 `Spring`으로 `JUuit Test`을 해보았다면, `Bean`을 활용하기에, 새로운 Spring container를 만들어 주면 쉽게 해결할 수 있고, 
- 만약 `Bean`없다고 하더라도, `@BeforeEach` 애노테이션에서  `new` 생성자를 사용하여, 인스턴스를 만들어 주면된다. 
- 하지만, 위와 같이 코딩테스트의 경우, main()메소드를 활용하며, `Bean`을 활용하지도 않는다. 
  - 게다가, 내부의 값들은 입력값(즉, Scanner와 같은 함수로 받아야하는 경우)을 활용하기에, `new`생성자도 불가능 하다. 
- 딱 내가 원하는 질문이 StackoverFlow에 있었다. 
  -  1.[Test a method without initialize the class](https://stackoverflow.com/questions/31587899/test-a-method-without-initialize-the-class)
  -  2.[Testing main method by junit](https://stackoverflow.com/questions/36349827/testing-main-method-by-junit)

