
# 

### 백그라운드에서 실행 
 - nohup java -jav [실행파일]
### 백그라운드에서 log 없이 실행 
 - nohup java -jav [실행파일] 1> /dev/null 2>&1 &

### 실행중인 PID 확인하기 
 - 리눅스 명령어
 - sudo lsof -t -i:8080 
   - 8080 포트로 실행하고 있는 프로세스 아이디를 찾음
 - sudo kill -9 [프로세스ID]
   - 해당 PID 강제종료

## Mysql 
 - foreign key constraint fails 에러
 - https://velog.io/@bigbrothershin/Mysql-foreign-key-%EB%AC%B4%EC%8B%9C%ED%95%98%EA%B3%A0-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0

### delete 요청
 

### Insert INTO 요청
