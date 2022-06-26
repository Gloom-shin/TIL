# Comparable<T> 인터페이스
Comparable 인터페이스는 `객체`를 `정렬`하는 데 사용되는 메소드인 `compareTo()` 메소드를 정의하고 있다.   
자바에서 같은 타입의 인스턴스를 서로 비교해야만 하는 클래스들은 모두 `Comparable 인터페이스`를 구현하고 있다.

- Boolean을 제외한 래퍼 클래스나 `String`, `Time`, `Date`와 같은 클래스의 인스턴스는 모두 정렬 가능하다.
  - String의 경우 사전순으로 정렬이 가능하며, `Time`과 `Date`도 시간순으로 정렬이 가능하다. 
- 이때 기본 정렬 순서는 작은 값에서 큰 값으로 정렬되는 오름차순이 된다.

### 예시
> 예를 들어, 자동차 스팩을 가지는 class를 만들고, 연식으로 정렬하고자 하는 상황을 들어보자 
 
### Car.java
  - 클래스에 `implements Comparable<Car>` 여기서 `<T>`은 `compareTo()` 메서드가 인자로 받을 `타입`을 말한다.
  - 인터페이스를 상속받은 것이기 때문에, `compareTo()` 메소드를 만들어 준다.
  - 메소드는 기본적으로 오름차순 정렬이며, 숫자가 클수록 뒤로 간다고 보면된다.
    - 즉, 들어오는 값보다 `this`의 값이 크면 큰수이기에 인덱스가 뒤로 밀린다. 
  ```
  public class Car implements Comparable<Car> {
	private String modelName;
	private int modelYear;
	private String color;
	Car(String mn, int my, String c) {
		this.modelName = mn;
		this.modelYear = my;
		this.color = c;
	}
	public String getModel() {
		return this.modelYear + "식 " + this.modelName + " " + this.color;
	}
	@Override
	public int compareTo(Car o) {
		if(this.modelYear == o.modelYear) {
			return 0 ;
		}
		else if(this.modelYear < o.modelYear ) {
			return -1;
		}
		else {
			return 1;
		}
	}
}
```
- Comparable01.java

```
public class Comparable01 {
	public static void main(String[] args) {
		Car car01 = new Car("아반떼", 2016, "노란색");
		Car car02 = new Car("소나타", 2010, "흰색");
		Car car03 = new Car("K5", 2014, "회색");
		Car car04 = new Car("티볼리", 2012, "파란색");
		Car car05 = new Car("제네시스", 2020, "검은색");
		Car[] arr = { car01, car02, car03, car04, car05 };
		Arrays.sort(arr); // 오름차순 정렬
		for (Car c : arr) {
			System.out.println("[ " + c.getModel() + "] ");
		}
		System.out.println("--------경계선--------- ");
		Arrays.sort(arr, Collections.reverseOrder()); // 내림차순 정렬
		for (Car c : arr) {
			System.out.println("[ " + c.getModel() + "] ");
		}
	}
}                              
```                                    
- int comparaTo(T o) 단 하나의 추상메소드를 가지고 있다.
### 결과값

<img src="https://user-images.githubusercontent.com/104331549/175807624-558f3f04-371d-438d-b7c3-96469f2a7f98.png">
 