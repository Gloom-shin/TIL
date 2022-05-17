Generic 은 사전적의미는 `일반적인` 혹은 `포괄적인` 이란 뜻이다.

# Generic 
- 클래스 내부에서 사용할 데이터 타입을 외부에서 파라미터 형태로 지정하면서 데이터 타입을 일반화 한다.
- 제네릭을 사용하면 가상의 자료형을 정의 후, 객체를 정의할 때 타입 매개변수를 선언하여 사용 가능

## Generic의 필요성
- 예를 들어
    - 어떤 class 안에 인스턴스 변수가 Object 타입을 가지고 있어서, Integer/String/Double/Char등 여러타입으로 값을 담을 수 있다. 
    - 하지만, 인스턴스 변수를 초기화 해주기위해선, 생성자 혹은 메소드를 거쳐야 하는데, 다양한 타입을 일일이 만들어줘야한다. 
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

### Generic 종류 

|Type argument|Detail|
|:--:|--|
|`<T>`|Type|
|`<E>`|Element|
|`<K>`|Key|
|`<V>`|Number|
|`<N>`|Value|
|`<R>`|Result|
