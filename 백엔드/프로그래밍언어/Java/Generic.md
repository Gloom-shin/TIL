Generic 은 사전적의미는 `일반적인` 혹은 `포괄적인` 이란 뜻이다.

# Generic 
- 클래스 내부에서 사용할 데이터 타입을 외부에서 파라미터 형태로 지정하면서 데이터 타입을 일반화 한다.
- 제네릭을 사용하면 가상의 자료형을 정의 후, 객체를 정의할 때 타입 매개변수를 선언하여 사용 가능

## Generic의 필요성
- 예를 들어
    - 어떤 class 안에 인스턴스 변수가 Object 타입을 가지고 있어서, Integer/String/Double/Char등 여러타입으로 값을 담을 수 있다. 
    - 하지만, 인스턴스 변수를 초기화 해주기위해선, 생성자 혹은 메소드를 거쳐야 하는데, 다양한 타입을 일일이 만들어줘야한다.(오버로딩이 필요!)
```
public class Box {
    private Object object;
    
    public Box(Interger integer) {this.object = integer; }
    
    public Box(String str) { this.object = str; }
    
    public Box(Double db) { this.object = db; }
    
    public Box(Char ch) { this.object = ch; }
}
```

  - 메소드와 생성자를 애초에 Object타입으로 만들어줘서 어떠한 타입이 들어와도 가능하게끔 만들 수도 있지만,
  - 이와 같은 경우 인스턴스 객체를 만들었을때, Object보다 하위 클래스 타입인 경우에는 `다운캐스팅`을 해줘야한다.

```
public class Box{
    private Object object;
    
	  Box(Object object) {
    		this.object = object;
  	}

	  public void set(Object object) {
	  	this.object = object;
	  }

  	public Object get() {
	  	return object;
	  }    

}    
```
```
public class Main {
	public static void main(String[] args) {
		Box box = new Box("String");
    
		String a = (String) box.get();  // 다운 캐스팅 사용
    
		Integer b = (Integer) box.get(); 
    //문자열을 정수로 강제타입변환할 수 없기때문에 ClassCastException이 발생합니다.
	}
}
```
 - 위와 같은 문제점들 때문에, 자바5부터 Generic이 탄생했다.
### 문제점 정리 
> 오버로딩은 번거롭고, 다운캐스팅은 리스크가 있다.   
> 하지만 이것은 제너릭이 해결해준다.


### Generic 종류 
 - Generic에서 T,E,K,V 는 이름만 다르지 근본적인 기능을 동일하다. 
 - 네이밍 관습으로, 가독성을 높히기위해 분류 하였다고 한다.

|Type argument|Detail|
|:--:|--|
|`<T>`|Type|
|`<E>`|Element|
|`<K>`|Key|
|`<V>`|Value|
|`<N>`|Number|
|`<R>`|Result|


## Generic 사용법
- 클래스를 만들 때 주로 사용된다.(메소드에서도 쓰이긴 한다.)
- 제네릭 클래스는 <>안에 있는 변수명을 타입 매개변수 혹은 타입 변수라고 한다. 
- 변수의 이름과 타입의 이름을 구분하기 위해 `하나의 대문자` 로 작성한다. 
- 여러개의 타입 매개변수를 가질 수 있다.   

\[ List.java \]
```
public interface List<E> extends Collection<E>{
   ...
}
```
- 위 `List.java` 파일의 선언부 `List<E>`를 보면 `List<>` 안에 "E"요소로 넣을 수 있다.

### 와일드 카드 
- `<?>`  타입 매개변수에 모든 타입을 사용 할 수 있다.
- 대체 뭐하는 녀석인지 모르겠다...
- 좀더 알아보니, 와일드 카드는 제한 기능을 활용하여 보다 논리적인 오류까지 잡아낼수 있는 힘이 생긴다고 한다.  
- 그럼 제한하는 방법에 대해 알아보자


