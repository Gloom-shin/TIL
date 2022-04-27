# HTML
- HyperText Markup Language
- 웹페이지를 구성하는 마크업 언어로 웹페이지의 뼈대 역할을 해준다. 
  - 여기서 마크업 언어이란? `태그`를 이용하여 데이터 구조를 만드는 언어를 말한다.  

<br></br>

 
## 사용법
 - <> 부등호를 사용하는 것을 Tag라고 하는데,  Tag로 시작해서 가운데 Content를 넣고 Tag로 끝난다. 
 - \<head\> , \<title\> , \<body\>, \<h1\>, \<div\> , \<p\> 등 많은 태그들이 있다. 
 - 구조는 트리 구조형태로 사용한다.
 - 태그 관련하여 더 상세한 내용들은 필요할 때마다 링크를 통해 찾아보자! [HTML 튜토리얼](https://www.w3schools.com/html/default.asp)
  
  
  <br></br>
 <br></br>
 ---
# CSS  
 - Cascading Style Sheets
 - 웹페이지를 꾸미는 역할 
    - 쉽게 말해 꾸미는 역할이지, 본업으로 들어가면 유저들에게 이해하기 쉽게 가독성을 좋게 늘려주는 역할이다. 
  
<br></br>

## 사용법
 - HTML에서  "style" 이라는 태그로 css를 불러 올 수 있다.
 - 사용법으로는 크게 3가지가 있다. 
    1.  외부 스타일 시트
    2.  내부 스타일 시트
    3.  인라인 스타일 

### 외부 스타일 시트
 - 가장 많이 사용되고 선호되는 방법으로 HTML파일 외부에 css파일을 별도로 만들고 불러오는 방식이다. 
 - 불러오는 방법은 html파일에서 \<link\> 태그를 사용한다.     
   ` <link rel="stylesheet" href="css파일명.css" />`

 - 이 방식으로 하나의 CSS파일이 아닌 여러개의 css파일을 불러올 수 있다.


### 내부 스타일 시트
 - 굳이 css파일을 만들지않고 html 파일 내부에 css을 작성할 수 있다. 
 - css 작성하는 방법은 \<style\>태그와 \</style\> 태그 사이에 입력하면 된다. 
 ```
 <style>
    ul{
          color : blue
    }
 </style>
 ```
 
 ### 인라인 스타일
  - 스타일 시트를 사용하지 않고 스타일을 적용할 대상에 직접 입력하는 방법
  - css사용방법은 태그에 style 속성을 이용한다.    
   `<div style ="color : deeppink"> 내용 어쩌구 </div>`
   
 ## 문법 
  - css는 각 태그별로 각기 다른 속성을 넣어줘야하기 때문에, 태그를 가르키는 방법이 문법이다. 
  - 대표적으로 id, class가 있다. 
  - 그외에 자세한 내용은 잘 정리된 셀렉터 링크를 참고 하자! [CSS Selectors](https://www.w3schools.com/cssref/css_selectors.asp)
 
