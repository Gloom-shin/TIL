## 브랜치 만들기 
> git branch "branchname"

### 예시 
 -  `git branch refactGloom` 입력하면, **새로운 브랜치**를 만든다.
 - `git branch` 만 입력하면, 현재 생성되어 있는 **브랜치들을 출력**한다.
 
 <img src="./images/gitBranch1.png">


## 브랜치 전환하기
 - 하지만, 위의 사진을 보면 `* main` 으로 되어 있고,새로운 브랜치앞엔 아무것도 없는 걸 알 수 있다.
 - 이 별`*`은 현재 선택된 브랜치를 말한다.
 - 그럼 새로운 브랜치를 선택하려면 어떻게 해야될까? 

 > git checkout "branchname"

### 예시 
 <img src="./images/gitBranchCheckOut.png">
 
  - 꿀팁으로, 브랜치를 생성하고 전환하는 작업을 한번에 실행할 수도 있다. 
> git checkout -b "branchname"

 <img src="./images/gitBranchOneTake.png">
