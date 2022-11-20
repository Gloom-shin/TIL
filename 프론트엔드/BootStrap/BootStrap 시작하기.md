# Bootstrap
 - 부트스트랩은 트위터에서 시작된 오프소스 프론트엔드 프레임워크이다.
   - 처음 시작은 트위터에서 사용하는 각종 레이아웃, 버튼등의 디자인과 기능을 CSS와 JavaScript로 만들어 놓은 것이다.
   - 점점 반응을 받아 현재는 웬만한 웹페이지를 미리 지정된 부트스트랩의 CSS클래스나 JavaScript 함수를 불러와 순식간에 만들수 있게 되었다.
 - 즉, 이미 만들어진 것을 가져와 쓴다는 것이다.
   - 물론 오픈소스 형태여서 커스텀하여 쓸수도 있다.
> 언제든지 불러와 쓸수 있는 링크를 태그 시켜놔야한다. 

<br>

# Bootstrap 시작하기
- Bootstrap에서 제공하는 `CSS`와 `JS`를 포함시켜주자
- CSS는 `<head>` 영역에 `<link>` 태그로 추가해주고 
- JavaScript 번들은 `<body>` 영역에 `<script>`로 추가해주면 된다.
  - 드롭다운, Popper 등 기능을 포함하고있다. [popper참고링크](https://popper.js.org/)
  - Bootstrao 3.0 시절만해도 popper.js가 굳이 필요없었지만 Bootstrap 4.0부터는 꼭 포함되어야 한다.
  - 보통 `</body>` 위에 적는다.
  
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
  </head>
  <body>
    <h1>Hello, world!</h1>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
  </body>
</html>
```


## CDN 링크 
 - Content Delivery Network의 약자로 콘텐츠 전송 네트워크라고 한다.
 - 말 그대로 이미 만들어 놓은 콘텐츠를 가져다 쓸수 있는 링크를 말한다.
 - 아래 링크를 들어가보면 만들어놓은 콘텐츠들의 코드를 볼 수 있다.
### CSS
 - https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css
### JS
 - https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js 

## 참고링크
 - [Bootstrap  공식 문서](https://getbootstrap.kr/docs/5.2/getting-started/introduction/#cdn-%EB%A7%81%ED%81%AC)

<br>
<br>

## 사용하는 법
- 공식문서에서 왼쪽 카테고리에서 원하는 것을 클릭하고
- 내가 만들고 싶은 것을 복사해서 `html`파일에 붙여넣기만 하면된다.
<img src="https://user-images.githubusercontent.com/104331549/202917919-3b405ba8-7b0f-4749-870d-6a10d8cc23d1.png">

### 붙여넣은 코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">

    <title>Bootstrap demo</title>
</head>
<body>
    <h1>Hello, world!</h1>
    <!--복사 붙여넣기 한 곳-->
    <button type="button" class="btn btn-outline-primary">Primary</button>

</body>
</html>
```

### 적용된 모습
<img src = "https://user-images.githubusercontent.com/104331549/202918039-212ea9be-7de5-4fc0-ac19-4978f98bcd2f.png">

 - 이렇게 매우간단하게 적용할 수 있다. 
 - 만약 추가적인 수정이 필요하다면 설정을 통해 바꿀수도 있다.
 - 또한, 버튼뿐만 아니라 카드, 그리디, NaviBar등 다양하게 제공하고 있다.
