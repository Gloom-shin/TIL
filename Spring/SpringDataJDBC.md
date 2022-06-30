# 데이터 Access 기술 
> Spring의 데이터 접근 기술을 이야기하면 JDBC와 JPA가 주로 나오는데, 여기서 JDBC의 뜻은  
> Java Database Connectivity 으로 자바에서 데이터베이스에 접속할 수 있도록하는 자바 API이다.    
> 즉, Spring만의 기술이 아니라는 것이다.


# JDBC란? 
 - 원래 데이터베이스에 데이터를 저장하고, 수정하고와 같이 사용하려면, 이에 맞는 코드레벨을 사용해야되지만, Java 코드에서 사용할 수 있도록 해주는 해주는 API이다
 - Java에서 제공하는 표준 기술이다. 
    - 초창기(JDK1.1)버전부터 제공되어 왔다. 
 - JDBC API를 이용하여 다양한 벤더(Oracle, MS SQL, MySQL)등의 데이터베이스와 연동 할 수 있다. 

> 여기서, JDBC 기술이 오래되었다는 걸 알 수 있듯이, 현재는 실무에서 보다 좋은 기술로 발전시켰고, JDBC API는 잘 사용하지 않는다고 한다.   
> 하지만, 어떻게 동작하는지 흐름을 기반으로 발전시켰기 때문에, 흐름정도를 알고 가자  

<br></br>

## JDBC 동작원리

<img src ="https://user-images.githubusercontent.com/104331549/176591350-9953841c-461d-4983-aad9-c9fce15b7431.png">
