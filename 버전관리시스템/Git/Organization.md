# Organization 
- 깃허브를 프로필을 보면 오른쪽 하단에 `organization`이라는 메뉴창이 있다.

<img src= "https://user-images.githubusercontent.com/104331549/171563024-661eef5d-bd27-4d55-ad71-230251c70446.png">

- 해당 이미지를 클릭하게 되면,  조직(그룹) 레포지토리이 나오게 된다. 
- 이것은 개인의 레포지토리가 아닌, 조직에서 새로 생성된 레포지토리이다. 
- 그래서 조직(그룹)관리자를 포함하여, 구성원들이 각자 `fork`와 `PullRequest`를 통해 관리하는 레포지토리가 되는 것이다.

### `Organization fork`
<p align = center><img src= "https://user-images.githubusercontent.com/104331549/171565064-30bc9f11-6386-4225-b743-f40584a3f8cf.png" width =60%></p>

### `Organization Pull Request`

<p align = center><img src= "https://user-images.githubusercontent.com/104331549/171565074-5b075f60-ed1c-4427-bf35-79700b69e2ec.png" width =60%></p>


그럼 **관리자**가 `repository`를 만들어 관리하는 경우와, `Organization`을 만들어 **관리자가 관리**하는 경우는 뭐가 다를까?

## Organization Repository과 개인 Repository
- 위와 같이 관리자(Owner)가 1명이고, 구성원(Member)가 2명인 경우에는 개인 Repository에서 관리해도 상관없다. 
- 오히려 Organization Repository는 관리자도 수정사항을 PR로 날려야 하기에, 관리자가 스스로의 수정이 자주 있다면 개인 Repository가 훨씬 편할 것이다.
- 하지만, 프로젝트의 규모가 커지거나, 혼자서 관리하기 어려워진 상황이 되었을 때는 Organization Repository를 사용한다. 

##  Organization Repository 장점 
### 1. 구성원에게 **원하는 권한**을 줄 수가 있다.

<p align = center><img src= "https://user-images.githubusercontent.com/104331549/171567089-6ce4cf64-12ff-496c-b2d0-ace6a824d0c8.png" width =60%></p>

##### 출저 : git 공식페이지
 - 위와 같이 Organization 내에 레포지토리의 종류는 공용 레포지토리랑, `Backend` 레포지토리랑, `Frontend` 레포지토리가 있다.
 - 팀 구성원은 관리자, 프론트엔드개발자, 백엔드개발자가 있다. 
 - 구성원에서 프론트엔드개발자는 `Frontend` 레포지토리는 수정할 수 있는 권한을 있으나, `Backend` 레포지토리는 읽을 수만 있게 권한을 부여해 줄 수 있다.
 - 구성원에서 백엔드개발자는 `Backend` 레포지토리는 수정할 수 있는 권한을 있으나, `Frontend` 레포지토리는 읽을 수만 있게 권한을 부여해 줄 수 있다.

### 2. 프로젝트 관리가 용이하다. 
  - 1번의 장점만으로도, 각자의 접근 권한(Team)이 다르고, 관리자를 여럿으로 둘 수 있기 때문에
  - 혼자서 관리하는 개인 Repository보다 Organization Repository가 훨씬 관리하기 용이핟.
  - `@Mention` 기능을 사용할 수 있어, 접근권한 나눈대로 팀(Teams)대로 멘션이나 문의가 가능하다. 
  - 그래서 개발자뿐만 아니라, 다른 연관된 파트 분들도 같이 구성원으로 관리가 가능하다. 
 
