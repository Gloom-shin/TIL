# git Rebase 
- git에서 브랜치를 합치는 방법으로는 두 가지가 있다.
    - [Merge](https://github.com/Gloom-shin/TIL/blob/main/%EB%B2%84%EC%A0%84%EA%B4%80%EB%A6%AC%EC%8B%9C%EC%8A%A4%ED%85%9C/Git/git%20branch.md)
    - Rebase     

## Rebase의 기초
 - 먼저 두 브랜치를 합치는 방법이기에, 상황이 브랜치가 두개로 나눠진 상황을 만들어 보자 
 - 보통의 방법이라면,  `Merge`를 사용하여 3-way-Merge를 통해 새로운 커밋을 만들어 낼 것이다. 


 - 하지만 Rebase는 비슷한 결과를 만드는 방식으로 C3에서 변경된 사항을 `Patch`로 만들고 이를 다시 C4에 적용시키는 방법이다. 
 - rebase명령으로 한 브랜치에서 변경된 사항을 다른 브랜치에 적용 할 수 있다. 
 - 
1. 

### 참고자료 
 - [git 공식홈페이지](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
