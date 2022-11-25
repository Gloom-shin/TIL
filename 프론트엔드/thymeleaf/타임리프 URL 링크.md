# 타임리프 URL 링크
- 타임리프에서 URL을 생성할 때
- `@{...}`문법을 사용하면 된다.
- 또한 적용할 수 있는 방법은 크게 2가지이다.
  - URL의 쿼리 파라미터로 넣어주는 방법
  - URL의 path-variable로 경로에 들어가는 방법

## 예시

### controller 
 - 파라미터로 사용할 변수 2개를 `model`에 `addAttribute`해준다.
```java
public class BasicController {
    @GetMapping("/link")
    public String link(Model model) {
        model.addAttribute("param1", "data1");
        model.addAttribute("param2", "data2");
        return "basic/link";
    }
}
```

### HTML
 - 총 4가지 방법으로 URL를 표현할 수 있고, 방법은 아래와 같다.
   1. 기본 URL이다. "@{/hello}" 이므로, `도메인/hello` 로 이동한다.
   2. 쿼리파라미터를 전달하는 URL이다. 전달방식은 get방식을 사용하며, "@{/hello(내가원하는 파라미터명1= ${변수}, 내가원하는 파라미터명2= ${변수}...)}"
      - 내가 원하는 파라미터 갯수만큼 변수가 있다면 넣은 만큼 파라미터로 전달 된다.
   3. path-variable URL이다. URL 링크안에 변수가 포함된 형태로 "@{/hello/{변수명1}/{변수명2}(변수명1 = ${변수}, 변수명2 = ${변수})}"
      - `()`안에 들어가는 변수와 URL에 들어가는 `{/hello/{변수}}`변수의 값은 같아야 대입되서 들어간다. 
   4. 마지막은 2번과 3번을 합친 형태이다. 
      - URL의 들어가있는 변수의 갯수보다 `()`에 들어가있는 변수의 갯수가 많다면 많은 만큼 쿼리파라미터로 전달된다.
```html
<body>
    <h1>URL 링크</h1>
    <ul>
        <li><a th:href="@{/hello}">basic url</a></li>
        <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query
            param</a></li>
        <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
        <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
    </ul>
</body>
```
### 실제 웹브라우저 소스코드 창
<img src="https://user-images.githubusercontent.com/104331549/203548834-9c38be28-f203-47f3-9f4f-8441c795081e.png">

 - &amp; = `url`에 들어가면 &로 표기된다. 


<br>
<br>

### 상대경로 절대경로 
 - 현재위치를 기준으로 상대경로도 표현할 수 있다. 
 - 상대경로 : `hello`
 - 절대경로 : `/hello`

#### 상대경로 결과 확인
<img src="https://user-images.githubusercontent.com/104331549/203550169-d50ad3f2-4b25-4440-973c-af956b44adbc.png">