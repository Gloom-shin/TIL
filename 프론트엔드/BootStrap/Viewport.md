
# Viewport
 - Bootstrap을 시작하면서 간단하게 사용법과 무엇인지 익히기위해 빠른시작을 보는데, 첫 시작에 
   - "프로젝트 최상위 폴더에 `index.html` file파일을 생성해주세요"
   - "모바일에서 반응형 동작을 휘해  `<meta name="viewport">`를 넣어주세요" 라고 나온다.
   - 또한 링크도 같이 태그가 되어 있는데, [Viewport meta tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag) 가 나온다.
> 그럼 대체 viewport가 무엇일까?

## 정의 
 - 현재 화면에 보여지고 있는 영역을 의미한다.
 - 기기 별로 Viewport가 다르기 때문에, 동일한 웹페이지라도 기기에 따른 배율 조정이 발생해 화면의 크기가 다르게 보이는 현상이 나타난다.
   - (왼)"웹 브라우저 환경" , (오)개발자도구로 바꾼 "모바일 환경"
<p align="center"><img src= "https://user-images.githubusercontent.com/104331549/202915705-f12305b1-40f2-4f79-a05c-75ede022a401.png" width="70%"></p>

 - 기존 PC
   - 원래기존의 뷰는 PC화면만을 위해 설계가 되어.웹브라우저라는 소프트웨어를 통해 페이지 크기를 조절해가며 웹을 조절할 수 있었다. 
   - 그러니까, 웹페이지 전체(풀 브라우징)가 보여져도 불편함이 없었다.
 - 모바일 및 태블릿
   - 작은 화면에 웹페이지 전체를 표시하게되면, 화면을 보기 어려웠다. 
   - 작은 화면의 모바일 단말기에 웹 페이지 모두를 표시하려고 하니 전체적인 페이지의 배율이 조정되어 작게 보이는 것이다.
 - 그래서 필요한게 `viewport`로
   - 설정하면 다양한 모바일 기기에서도 페이지의 너비나 화면 배율을 설정할 수 있다.

### 사용방법
 - head 태그 사이에 코드로 입력한다.
   - ```html<meta name="viewport" content="width=device-width, initial-scale=1.0">```
   - 적용한 화면
   - <p align="center"><img src= "https://user-images.githubusercontent.com/104331549/202915849-d149ad42-9a16-4057-866e-bc460eb4c51c.png" width="70%"></p>

  - 기본적으로 데스크탑 브라우저에서는 `viewport 메타 태그`를 사용하지 않기 때문에 무시된다.
   - 즉, 모바일 환경에서만 적용된다는 것이다.


### viewport 속성
#### width : viewport의 가로 크기를 조정한다.
   - 숫자값이 들어갈 수 있다. 
   - `device-width`와 같은 특정한 값을 사용할 수도 있다. 
     - `device-width`는 100% 스케일에서 CSS 픽셀들로 계산된 화면의 폭을 의미합니다.

#### height : viewport의 세로 크기를 조정한다.
   - 모바일은 스크롤을 사용하여 위아래로 이동하기 때문에 크게 건들릴 필요는 없다.
####  initial-scale : 페이지가 처음 로딩될 때 줌 레벨을 조정한다.
   - 값이 1일때는 CSS 픽셀과 기기종속적인 픽셀 간의 1:1 관계를 형성
####  minimum-scale : viewport의 최소 배율값
   -  기본값은 0.25
####  maximum-scale : viewport의 최대 배율값
   - 기본값은 1.6
####  user-scalable : 사용자의 확대/축소 기능을 설정
   - 기본값 true

<br>

# Bootstrap 시작하기
- 이쯤에서 다시 돌아와서 Bootstrap 시작하기의 처음 코드를 보면
- 아래 html코드가 이해가 될 것이다.
```html
<!doctype html>
<html lang="en">
<head>
   <meta charset="utf-8">
   <!-- 추가해야될 중요포인트 -->
   <meta name="viewport" content="width=device-width, initial-scale=1">  
   <title>Bootstrap demo</title>
</head>
<body>
<h1>Hello, world!</h1>
</body>
</html>
```

 - 그럼 이제 반응형 웹을 만들 준비가 된 것이다.

