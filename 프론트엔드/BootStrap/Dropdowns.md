# 드롭다운
 - 개인프로젝트에서 게시판를 진행하던 도중, 게시판 생성할 때 사용자가 카테고리를 선택해야되는 기능을 만드려고한다.
   - 카테고리는 사용자가 임의로 정할 수 없고, 이미 정해진 리스트중에 선택을 해야하기 때문에 "드롭다운 박스"로 구현하고자 했다.
   <img src= "https://user-images.githubusercontent.com/104331549/205267031-33c39ada-d0cc-49ee-af94-8e3a0d329d56.png">

 - 프론트엔드 개발을 부트스트랩으로 처리했기때문에,
    - 드롭다운박스 역시, 부트스트랩 홈페이지에 있는 컴포넌트를 가져와서 쓸 예정이었다.
 - 하지만, 예상했던 결과대로 나오질 않았다. 

<br></br>

## 에러발견

### 적용 코드 
 - 부트스트랩 문서에 있는 Dropdowns 첫번째 `single button` 코드를 그대로 가져온 것이다.
```html
<div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
        Dropdown button
    </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
    </ul>
</div>
```

### 결과 

<img src="https://user-images.githubusercontent.com/104331549/205268746-26e05083-8bab-464b-8711-c5da78a1fcf3.gif">

 - 버튼을 눌러도 아무런 버튼 색만 변하지, 드롭박스가 생성되지않았다. 

<br></br>

## 원인
 - 가장먼저 공식문서를 제대로 읽어보았다.
<img src ="https://user-images.githubusercontent.com/104331549/205270662-e0661e26-2955-4982-bfb7-27a05e237836.png">

 - 첫 문단은 간단하게 드롭다운에 대한 설명이었고, 
 - 두번째 문단 부터가 사용 세팅 방법에 대한 설명이었다. 내용은 아래와 같다.
   - 드롭다운은 동적으로 움직이는데, 뷰포트와 같이 동작할 수있는 타사 라이브러리 Popper를 기반으로 동작한다.
     - 그래서, `popper.min.js` 를 라이브러리에 추가시키거나 
     - `popper.min.js`가 포함된 `bootstrap.bundle.min.js`를 추가 시키면된다.

### 요약 
 - 두가지 방법으로 해결할 수 있다. 
 - bootstrap.bundle.min.js를 다운받아 추가해준다.  
   - `<script th:src="@{/../assets/vendor/bootstrap/js/bootstrap.bundle.min.js}" src="/../static/assets/vendor/bootstrap/js/bootstrap.bundle.min.js"></script>`
 - 외부에서 불러와 사용한다.
   - `<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>`

## 문제해결 결과
<img src="https://user-images.githubusercontent.com/104331549/205272747-ec17c488-b287-41cf-8b57-8058b2564bf6.gif">