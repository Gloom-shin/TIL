# API 문서란?
 - 보통 백엔드 애플리케이션은 REST API 방식의 애플리케이션이다.
 - 클라이언트는 백엔드 애플리케이션에 정보를 보내고 응답을 받기위해선, **보내야하는 요청정보를 알아야** 제대로된 요청을 할 수 가 있다.
 - 그 요청 정보를 문서로 잘 정리해 놓은 것을 API 문서 혹은 API 스펙(사양)이라고 한다. 


## API 문서 생성의 자동화
 - 만약  API 문서를 워드나 노션, 에버노트등의 문서를 이용하여 직접 수기로 작성한다면 어떨까요?
 - 먼저, 문서의 유지보수 관리가 어렵다. 기능이 추가되고 수정될때 마다 변경해야되며, 수기로 직접 작성이니 실수가 발생 할 수도 있다.

> 수기로 직접 작성해야되는 것은 너무나도 비효율적이다.   
> 그래서 문서 자동화를 통해 API문서의 완성도를 높일 수 있다.  
 


## Spring Rest Docs
- 대표적으로 API 문서 자동화라고 하면,`Swagger` 와 `Spring Rest Docs`가 나온다.
- Swagger는 API 동작을 테스트하는 용도에 더 특화되어 있다면, 
- Spring Rest Docs 은 깔끔 명료한 문서를 만들수 있다.
- 이 둘을 비교하자면 아래와같다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/179381213-717106e1-7889-47c4-8f4f-76ef14dfe781.png"></p>


### Swagger
> 잠깐 Swagger에 대해 좀더 자세히 알아보자면

- Java 기반의 애플리케이션에서는 전통적으로 쓰인 API 문서 자동화 오픈소스이다. 
- 애너테이션을 기반으로 API 문서를 만드는 도구로, API에 필요한 클래스마다 추가해줘야한다. 
- 그렇기 때문에, 무수히 많은 애너테이션들을 추가해야되고, 엔드포인트를 위한 기능 구현 코드가 한눈에 들어오지도 않게된다. 
- 하지만, 좋은 점은 API 테스트를 해볼 수 있는 화면이 제공된다는 것이다. 
    - 또, 이말은 API 문서를 만들때에는 테스트과정없이 만들 수도 있다는 말이 되어, 자칫 실수하더라도 그대로 적용되어 API문서가 작성된다는 것이다.


###  Spring Rest Docs의 API 문서화 방식
 > Swagger에 있었던 단점들을 Spring Rest Docs는 어떻게 할까?
 
 - 먼저 가장 큰 차이점은 **실제 기능이 구현된 코드**에는 API 문서 생성을 위한 애너테이션 같은 어떠한 정보도 추가되지 않는다.
 - 대신에 테스트 코드에서 API 문서를 위한 정보를 추가한다. 
 - 게다가 테스트 문서가 성공해야지만, API 문서가 작성이 되기 때문에, Controller에 정의 되어있는 요청과 응답들이 API 스펙정보와 일치하는 문서가 만들어 진다는 것이다.
    - 즉, 실수를 줄이고, 자칫 일어날 수 있는 사고를 미연의 방지해준다.
 - 하지만, API 툴로써 기능제공은 따로 없다. 그냥 문서이다.