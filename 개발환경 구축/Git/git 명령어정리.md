#  Git 명령어 정리  
### clone 
-
### status 
-
### restore 
 - 아직 add 되지않은 Local Repository의 변경 사항을 폐기할 수 있다.(add 된 파일은 폐기되질않는다.) 
   - `git restore 파일경로/파일명`
### add
- 내 로컬의 untracked file을 Git의 관리하인 Staging Area로 추가해 준다.
   - `git add 파일명`으로 원하는 파일을 추가할 수 있다.
   - `git add .` 혹은 `git add -a`으로 모든 파일을 한번에 추가 할 수 있다. 
### reset 
- git add 로 파일이 Staging Area에 들어가있는 경우, `git reset HEAD 파일명` 을 통해 git add 를 취소할 수 있다.
   - 위 명령어를 수행하면, Staging Area에서 unstaged로 옮겼을 뿐, 수정된 사항은 바뀌지않는다. 
   - HEAD외에 main, origin/HEAD , origin/main 이 있다.
### commit 
- Staging Area 에서 Local Repository로 추가해주는 명령어로, 변경 사항을 저장한다.
   - 저장할때 간단한 메모를 남겨, 어떻게 변경되었는지 기록하고 관리할 수 있다.
   - 가장 많이 쓰는 명령어는 `git commit -m "메시지내용"` 이다. 
   - 여기서 메모하는 내용은 변경 기록내용을 봐야하기에, 쓰는 요령(컨텐션)이 따로 몇몇 있다. 
### reset HEAD^ 
- 
### push 
- 
