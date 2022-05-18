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
- 각 설명은 사전 오름차순으로 배열된다.

### interface 
  **1. Collection<E>** ⭐ 
  - collection 계급 인터페이스이다.
  
  **2. Comparator<T>** ⭐
  - 개체 Collection에 총 순서를 부여해주는 비교 함수이다.
  
  **3. Deque<E>** ⭐
  - 양 끝에서 Element 삽입 및 제거를 지원하는 선형 인터페이스이다.   
  
**4. Enumeration<E>** (iterator 사용권장)   
  - 단방향 순환 인터페이스이며, iterator의 하위버전이다.
  - 자바 초기버전부터 존재한 컬렉션프레임워크
  - 기본적으로 값을 보존하여 스레드에 안전한 구조로 사용할 때 사용한다.
 
**5. EventListener**	  
 
**6. Formattable**	   
 
**7. Iterator<E>** ⭐  
  - 단방향 순환 인터페이스이며, 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하여 쓰인다.
  - `remove()`메소드가 존재하여, 공간 낭비를 최소화 할 수 있다. 
  - JDK1.2 이후로 추가된 컬렉션프레임워크
  
 **8. List<E>**	⭐  
  - 순서있는 컬렉션(시퀀스라고도 함)
  - 기본적으로 인덱스를 가지고 있는 객체 배열이다.
 
 **9. ListIterator<E>**	  
  - 양쪽 방향으로 이동할 수 있는 `iterator`이다. 
  - `iterator` 하위클래스 이다. 
 **10. Map<K,V>** ⭐  
  - `Key`값과 `Value`값이 매핑되어 있는 객체이다.
  - `Collection<E>`에 포함되지 않는다는 걸 유의해야한다.
  - 게다가 `Map`인테페이스는 부모인터페이스가 없다.
    - 참고 링크 (Map인터페이스는 부모 인터페이스가 있나요?)[https://stackoverflow.com/questions/40970695/which-is-the-parent-class-of-java-util-map-interface]
 
 **11. Map.Entry<K,V>**  
  - 키와값 쌍을 한번에 저장합니다.
  - 예를 들어
     - `Map<K,V>`의 경우 `K`값과 `V`값을 같이 출력하려면, `keySet()`를 통해 `K`값을 추출하고, 다시 `get(key)`로 `V`를 찾아 같이 출력해야된다.
     - `Map.Entry<K,V>`의 경우 `K`값과 `V`값을 같이 출력하려면, `K`값은 `getKey()`으로 `V`값은 `getValue()`로 출력하면 된다.
 
 **12. NavigableMap<K,V>**  
  - 정렬된 `Map<K,V>`으로 `TreeMap<K,V>`구현체로 생성하는 편이다.
  - `SortedMap<K,V>`인터페이스를 상속 받는다.
 
 **13. NavigableSet<E>**  
  - 정렬된 `Set<E>`으로 TreeSet 구현체로 생성하는 편이다.
  - `SortedSet<E>`인터페이스를 상속 받는다.

 **14. Observer**  
 **15. Queue<E>** ⭐  
  - 먼저 저장된 값이 먼저 추출되는 인터페이스이다. 
  - 보통 `LinkedList<E>` 구현체로 생성하는 편이다. 
 
 **16. RandomAccess**	  
 **17. Set<E>** ⭐  
  - 중복 요소를 포함하지 않는 컬렉션이다. 
 
 **18. SortedMap<K,V>**  
  - `K`값의 전체 순서를 추가로 제공하는 `Map<K,V>`인터페이스이다.
 
 **19. SortedSet<E>**	  
  -  요소의 전체 순서를 추가로 제공하는 `Set<E>`인터페이스이다.
 
---
 ### Class
 - 워낙내용이 많아 생략된 부분도 있으니 참고 바란다.
 - 인터페이스가 구현하지 못한 부분을 Class를 통해 구현하여, 구현체라고도 한다.
  
#### 1. AbstractCollection<E>
 - 이 클래스는 컬렉션 인터페이스를 구현하는데 필요한 최소한의 골격을 구현한다.
 - 컬렉션 구현체의 가장 근본이되는 클래스라고 볼 수 있다. 
 
#### 2. AbstractList<E>
 - `random access 데이터 store`에 의해 지원되는 인터페이스를 구현하는데 필요한 최소한의 List인터페이스를 구현한다.
 - 모든 List 구현체의 상위 클래스가 된다.
#### 3. AbstractMap<K,V>
 - Map 인터페이스를 구현하기위해 최소한의 골격을 제공하는 클래스
#### 4. AbstractMap.SimpleEntry<K,V>
 - Key값과 Value값을 유지하기위한 클래스
 - 이 클래스 덕에 Value값을 변경할 수 있다.
#### 5. AbstractMap.SimpleImmutableEntry<K,V>
 - 변경할 수 없는 Key값과 Value값을 유지하는 클래스
 - setValue()를 지원하지않아, 값을 바꿀 수가 없다.
#### 6. AbstractQueue<E>
 - Queue 작동을 하기위해 최소한의 골격을 제공하는 클래스
#### 7. AbstractSequentialList<E>
 - `sequential accessList 데이터 store`로 이루어진 List인터페이스를 구현하기 위해 최소한의 골격을 제공하는 클래스
#### 8. AbstractSet<E>
 - Set 인터페이스를 구현하기위해 최소한의 골격을 제공하는 클래스
#### 9. ArrayDeque<E>
#### 10. ArrayList<E> ⭐
#### 11. Arrays ⭐
 - 정렬 및 검색과 같은 배열을 조작하는 다양한 방법이 포함되어 있는 클래스
#### 12. BitSet
#### 13. Calendar
#### 14. Collections ⭐
#### 15. Currency
#### 16. Date
#### 17. Dictionary<K,V>
#### 18. EnumMap<K extends Enum<K>,V>
#### 19. EnumSet<E extends Enum<E>>
#### 20. HashMap<K,V> ⭐
#### 21. HashSet<E> ⭐
#### 22. Hashtable<K,V> 
#### 23. LinkedHashMap<K,V>
#### 24. LinkedHashSet<E>
#### 25. LinkedList<E> ⭐
#### 26. PriorityQueue<E> ⭐
#### 27. Random
#### 28. Scanner
#### 29. Stack<E> ⭐
#### 30. StringTokenizer
#### 31. TreeMap<K,V> ⭐
#### 32. TreeSet<E>  ⭐
#### 33. Vector<E>


  
 ### 그 외 `Enum` , `Exception`, `Error` 생략

  
  
  
