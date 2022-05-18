# import java.util
코드를 작성하다보면, 내가 필요한 Source를 위해 패키지를 `import`해와야 하는 경우가 많다.    
그 중, 가장 많이 불러오는 패키지가 `import java.util.*` 이 아닐까 싶다.    
이번엔 대체 이 패키지가 무슨 패키지인지 알아보고자 한다.    


## java.util
- `util`(유틸)의 사전적 의미는 효용 이란 뜻으로, 좋은 결과를 내기 위해 만족감있게 쓰임(?) 한국어로는 너무 어렵다. 
- 오라클 공식 문서에 있는 내용은 아래와 같다.  [Package java.util](https://docs.oracle.com/javase/7/docs/api/java/util/package-summary.html)

> Contains the collections framework, legacy collection classes, event model,  
> date and time facilities, internationalization, and miscellaneous utility classes (a string tokenizer, a random-number generator, and a bit array).

- 해석해보면, "컬렉션 프레임워크, 레거시 컬렉션 클래스, 이벤트 모델 ,날짜 및 시간, 국제화 및 기타 유틸리티등를 포함합니다. "
- 말 그대로 코드를 작성하면서 유용한 내용은 다 들어가있다고 볼 수 있다.

- 그럼 이번엔 패키지안은 어떻게 구성되어 있는 지 알아보자(**물론 컬렉션 프레임워크 위주로 알아볼 예정이다.**)

## util 패키지 구성 
- 먼저 크게 패키지 구성은 5가지로 `interface` ,`class`, `Enum` , `Exception`, `Error`로 분류된다. 

### interface (오름차순 정렬)
  **1. Collection<E>**  
  - collection 계급 인터페이스이다.
  
  **2. Comparator<T>**
  - 개체 Collection에 총 순서를 부여해주는 비교 함수이다.
  
  **3. Deque<E>**
  - 양 끝에서 Element 삽입 및 제거를 지원하는 선형 인터페이스이다.   
  
4. Enumeration<E> (iterator 사용권장)   
  - 순환 인터페이스이며, iterator의 하위버전이다.
  - 자바 초기버전부터 존재한 컬렉션프레임워크
  - 기본적으로 값을 보존하여 스레드에 안전한 구조로 사용할 때 사용한다.
 
5. EventListener	   
6. Formattable	   
  **7. Iterator<E>**
  - 순환 인터페이스이며, 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하여 쓰인다.
  - remove()메소드가 존재하여, 공간 낭비를 최소화 할 수 있다. 
  - JDK1.2 이후로 추가된 컬렉션프레임워크
  
  8. List<E>	
  9. ListIterator<E>	
  10. Map<K,V>
  11. Map.Entry<K,V>
  12. NavigableMap<K,V>
  13. NavigableSet<E>
  14. Observer
  15. Queue<E>
  16. RandomAccess	
  17. Set<E>	
  18. SortedMap<K,V>
  19. SortedSet<E>	

  
  
  
  
  
