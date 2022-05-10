# StringTokenizer
- 문자열을 지정한 구분자(delim)로 문자열을 쪼개주는 클래스
- 쪼개어진 문자열을 **토큰(token)**이라 부른다. 


## 사용법 

사용법은 간단하다. 먼저 `import java.util.StringTokenizer`를 선언해주고
아래 생성자 중 하나를 사용하여, 사용할 수 있다. 
|생성자|설명|
|--|--|
|StringTokenizer(String str);|전달된 매개변수 str을 기본값으로 분리한다. 기본 delimiter는 공백문자들인 (스페이스)\t,(스페이스),(줄바꿈)\n 과 같은 이스케이프 시퀀스이다. |
|StringTokenizer(String str,String delim);|특정 delim으로 문자열을 분리한다.|
|StringTokenizer(String str,String delim,boolean returnDelims);|str을 특정 delim으로 분리시키는데, 그 delim까지 token으로 포함할지 결정. returnDelims이 true일시 포함, false일시 미포함(기본값)|


### 테스트 
```
String str = "show me the money something for nothing ";
StringTokenizer tokenizer = new StringTokenizer(str);
while(tokenizer.hasMoreTokens()){
      System.out.println(tokenizer.nextToken());
}
System.out.println("total tokens:"+tokenizer.countTokens());
```

### 출력값
> show   
> me   
> the  
> money  
> something  
> for  
> nothing  
> total tokens:0  
 - delim(구분자)를 별도로 지정해주지 않고, returnDelims도 지정해주지 않았기에, 기본값인 공백으로 구분했으며, 공백은 포함하지 않았다. 


### 테스트 2 
- 이번엔 구분자를 `, =` 로 두고 해보자
```
String str = "show me the, money,=something for=nothing ";
StringTokenizer tokenizer = new StringTokenizer(str,",=");

while(tokenizer.hasMoreTokens()){
    System.out.println(tokenizer.nextToken());
}
System.out.println("total tokens:"+tokenizer.countTokens());
```
### 출력값
> show me the  
> money  
> something for  
> nothing   
> total tokens:0  

- 신기하게 `,=`두개다 각각 분리되어 적용되며, 문자열이 쪼개진걸 볼 수 있다.

### 테스트 3 
- 그럼 마지막으로 ` returnDelims= true` 로 해보자
```
String str = "show me the, money,=something for=nothing ";
StringTokenizer tokenizer = new StringTokenizer(str,",=", true);  // 위에 코드와 동일하며, true만 추가됨 

while(tokenizer.hasMoreTokens()){
    System.out.println(tokenizer.nextToken());
}
System.out.println("total tokens:"+tokenizer.countTokens());
```

### 출력값
> show me the
> ,   
>  money  
> ,  
> =  
> something for  
> =  
> nothing  
> total tokens:0

- 구분자를 포함하면, 구분자 별도로 token값으로 나눠짐을 알 수 있다.




.
