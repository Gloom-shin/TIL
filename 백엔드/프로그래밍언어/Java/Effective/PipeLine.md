# 파이프라인? 들어본것 같은데? 👂
 - 개발자들계에서 생각보다 많이 들리는 말중 `파이프라인`이라는 말을 종종 듣는다. 
 - 나는 보통 아키텍처나 디자인패턴을 언급할 때 들었던것같고, 또한 어느 공간에서 원하는 장소로 데이터를 옮길때에도 쓰이는 말 같았다. 
 
 > 그럼 파이프라인은 무슨 뜻일까?
 > 
 <img src = "./images/pipeLineStory.jpg">

 <br></br>
 
 ## 파이프라인 우화이야기 👀
 - 파이프라인 관련해서 가장 유명한 우화이야기가 있다. 
 - 간단하게 여기서 이야기하자면 아래와 같다.

> 옛날 어느 마을에 두 젊은이가 살고 있었습니다.   
> 그러던 어느날 마을 이장님이 마을 반대쪽에 있는 산위에서 물을 길러와 마을 물탱크에 물을 넣는 일을 의뢰 했죠  
> 물론, 물을 가져온 만큼 돈을 받을 수 있었습니다.    

<img src = "./images/pipeLineStory1.png">

> 두 젊은이는 열심히 일을 했고 돈을 모으기 시작했습니다.  
> 그 중 한 젊은이는 좀 더 열심히 하면 원하던 것들을 구입할 수 있을거라고 생각했죠. 
> 하지만 다른 젊은이는 좀 더 쉽게 물을 가져올수 있는 방법을 계속 고민했습니다.  

<img src = "./images/pipeLineStory2.png">

> 그러다가 파이프라인을 구축하는 것을 생각해냈습니다.   
> 반대쪽 산위에서 마을까지 파이프라인을 구축하면 더이상 물통으로 물을 나르지 않아도 돈을 벌 수 있는 아이디어 였죠. 

<img src = "./images/pipeLineStory3.png">

> 다음날 젊은이는 다른 젊은이한테 같이하자고 계획을 이야기했지만, 
> 다른 젊은이는 거절하였죠, 왜냐하면 그일은 너무 힘들고 지금 당장은 돈을 벌지 못하기 때문이라고요.

<img src = "./images/pipeLineStory4.png">

> 결국 혼자서 파이프라인을 구축하기 시작햇습니다. 
> 물론 처음에는 직접 물통으로 나르던 젊은이가 돈을 잘 벌었죠. 
> 하지만 오랜 시간이 흘러, 나이가 들면서 예전만큼 물을 나를 수 없게 되었죠 
> 그동안 파이프라인을 완성 시킨 다른 이는 나이가 들어서도 계속해서 돈을 벌 수 있게 되었습니다.

<img src = "./images/pipeLineStory5.png">

<br></br>

## 우화 이야기후 느낌점 🤔
### 예전의 나
 - 예전에는 "남들과 다른 생각을 해야한다. 포기하지않고 계속 발전해야된다" 등의 `교훈의 의미`로 우화이야기를 받아드렸습니다.
 - 중간 부분을 생략해서 그렇지, 파이프라인을 구축하기위해 `혼자서 고통을 인내하는 모습`을 묘사한 부분도 있었거든요.

### 지금의 나 
 - 지금은 요점은 파이프라인의 기능에 초점을 맞춰야 될 것 같습니다. 
 - 기본적으로 `파이프라인`은 물을 길러온다 라는 기능이 있지요. 
    - 즉, 운반 수단입니다. 
 - 하지만, 물통에 직접 담는 작업도 운반수단중 하나입니다. 그럼 차이점이 무엇일까요??
    - 바로, `자동화` 입니다.
    - 개발에서의 자동화란, 매우 복잡하고 일일이 쳐야하는 코드를 한번에 처리할 수 있다는 뜻으로 해석할 수 도 있습니다. 
    - 예를 들어 
        - for문을 사용하여 순차적으로 처리해야는 일을 Stream으로 한번에 처리할 수 있게 된다는 거죠. 

[일반적인 운반]
```
double average;
int sum = 0;
int count = 0;
for(Person p : persons){
    if(p.getGender.equals("남자")){
         sum += p.getAge();
         count += 1;
    }
}
if(count == 0){
   average = 0;
}
average = (Double)sum/count;
```
[파이프라인 운반]
```
double average = 
members.stream()
            .filter(m -> m.getGender().equals("남자")) 
            .mapToInt(Member::getAge)
            .average() 
            .orElse(0.0);
```
<br></br>

## 소프트웨어 개발자의 파이프라인
 - 소프트웨어 엔지니어링 팀에서 파이프라인은 포괄적인 뜻을 지닌다. 
 - 개발자나 DevOps전문가가 효율적이면서도 확실하게 그들의 코드를 `컴파일`, `빌드` 그리고 `배포`하게 해주는 `자동화된 프로세스`를 묶어 이야기한다.
 - 물론 이래야 한다거나, 반드시 활용해야하는 도구를 지정하고 빠른 규칙은 없지만, 일반적으로 파이프라인이 가져야할 컴포넌트들이 있다. 
 - 일반적인 컴포넌트 
     - 빌드자동화
     - 지속적 통합
     - 테스트 자동화
     - 배포 자동화 


### 파이프라인 카테고리
- Source Control
- Build tools
- Containerisation
- Configuration Management
- Monitoring

> 소프트웨어 배송 파이프라인(Software Delivery Pipeline) 개념의 핵심은 어떤 파이프라인의 단계 사이에 수동적 단계나 수동적인 변경이 필요없는 자동화(automation) 이다. 

- 무언가 더 Deep하게 들어가면 디테일해질 것 같아, 현재는 여기까지 인지하면 좋을 듯하여 여기까지만 진행하겠습니다.
