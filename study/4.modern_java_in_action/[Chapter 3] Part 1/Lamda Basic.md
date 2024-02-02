## 1. 람다식에는 크게 두가지 표현 스타일이 있습니다.

- (parameters) -> expression

- (parameters) -> { statements; }

```java

() -> {}
() -> "Dabeen"
() -> {return "Mario;"}
(Interger i) -> return "Alan" + i; // 두가지 방법 되는지 확안
(String s) -> {"Iron Man";} // 두가지 방법 되는지 확인

```


## 2.람다식은  ~와  ~ 에 사용 될 수 있습니다.

람다식은 함수형 인터페이스 라는 문맥에서 사용 할 수 있습니다.
함수형 인터페이스는 추상 메서드가 하나만 가지고 있는 인터페이스를 뜻합니다.

> Q. SmartAdder 는 함수형 인터페이가 될 수 있을까요? 

```java
public interface Adder { 
	int add(int a,int b);
}
public interface SmartAdder extends Adder{
	int add(double a,double b);
}
```

아래의 인터페이스는 우리가 자주 사용 해왔던 낯이 익은 자바 API 의 함수형 인터페이스 입니다.

```java
public interface Predicate<T> {
	boolean test(T t);
}
public interface Runnable {
	void run();
}
public interface Comparator<T>{
	int compare(T o1,T o2);
}
```

> 참고 디폴트 메서드는 여러개를 가지고 있어도 됩니다.

### 그럼 람다식을 변수에도 사용해보고 파리미터로 전달도 해봅시다!

```java
Runnable r1 = () -> System.out.println("eunyoung");

Runnabe r2 = new Runnable() {
	public void run(){
		System.out.println("hyunki");
	}
};

public static process(Runnable r){
	r.run();
}
process(r1);
process(r2);
process(()-> System.out.println(yejin));

```

함수형 인터페이스 내 추상 메서드의 시그니처는 람다 표현식의 시그니처를 가리킵니다.
그리고 이 람다표현식의 시그니처를 서술하는 메서드를 함수 디스크립터 라고 합니다.

또한 함수형 인터페이스에 @FunctionalInterface 어노테이션을 추가하면 커스텀 함수형 인터페이스를 작성 할 때 추상메서드를 하나 이상 만드는 실수를 컴파일 단계에서 방지 할 수 있습니다.

## 3. 람다식을 활용하여 불필요한 중복 코드 줄이기

람다식의 가장 큰 특징이 곧 람다식의 장점 입니다.
동작을 파리미터화(변수에 할당) 하여 메서드에 전달 하여 중복된 코드를 줄여봅시다!

먼저 람다식을 활용하지 않은 코드 입니다.

```java
public String processFile() throws IOException {
	try(BufferedReader br = 
			new BufferedReader(new FileReader("data.txt"))){
		return br.readLine();
	}
}
```

위의 코드는 하나의 파일에서 한줄을 읽고 String 으로 반환 하는 메서드 입니다.
지금과 같은 코드에서 두줄의 코드를 한번에 읽어 들이기 위해서는 하나의 메서드를 추가로 작성해야하는 것이 불가피 합니다.
하지만 람다식을 활용하면 우리는 파일에서 읽는 과정을 파리미터화 하여 메서드를 다양하게 활용 할 수 있습니다.

여기서 processFile 메서드는 파일에서 데이터를 읽고 String으로 반환 하는 역할을 합니다.
우리는 여기서 몇줄을 읽을 것인지에 대한 부분을 파라미터화 하여 processFile 메서드를 다양한 곳에서 활용 할 수 있습니다.

먼저 processFile 에서 람다식을 사용할 수 있게 해야합니다.

```java
@FunctionalInterface
public interface BufferedReaderProcessor {
	String process(BufferedReader b) throws IOException;
}

public String processFile(BufferedReaderProcessor p) throws IOException {

}
```

process 추상 메서드의 반환형과 파라미터를 확인 하세요!
우리의 목표는 process 추상 메서드를 람다식을 통해 구현 하는 것입니다!

```java
public String processFile(BufferedReaderProcessor p) throws IOException{
	try(BufferedReader br = 
			new BufferedReader(new FileReader("data.txt"))){
			return p.process(br);
	}
}
```

맨 처음 코드와 달라진 점을 확인하세요!
우리는 이제 process 추상 메서드를 상황에 맞게 람다식으로 구현하여 processFile 메서드를 다양 하게 활용 할수 있게 되었습니다.

```java
String threeLine = processFile((BufferedReader br) -> br.readLine() +
						br.readLine() + br.readLine();
					);
```


## 4. 여러가지 함수형 인터페이스

예제를 살펴보며 여러가지 함수형 인터페이스를 살펴 봅시다.

### Predicate

```java
public interface Predicate<T> {
	boolean test(T t);
}
public <T> List<T> filter(List<T> list ,Predicate<T> p){
	List<T> results = new ArrayList<>();
	for(T t: list){
		if(p.test(t)){
			results.add(t);
		}
	}
	return results
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);

```

### Consumer

```java
public interface Consumer<T> {
	void accept(T t);
}
public <T> void forEacg(List<T> list ,Consuner<T> c){
	for(T t: list){
		c.accept(t);
	}
	return results
}

foeEach(
	Arrays.asList(1,2,3,4,5), (Integer i -> System.out.println(i) );
)

```

### Function

```java
@FunctionalInterface
public interface Function<T,R>{
	R apply(T t);
}

public <T,R> List<R> map(List<T list, Function<T,R> f){
	List<R> result = new ArrayList<>();
	for(T t: list){
		result.add(f.apply(t));
	}
	return result;
}

List<Integer> l = map(
			Arrays.asList("lamdas","in","action"),
			(String s) -> s.length()
			);
						
```

### 기본형 특화 함수형 인터페이스

위에서 살펴본 함수형 인터페이스들은 모두 제네릭을 활용하고 있어 참조형 변수만 다룰 수 있습니다.
위의 함수형 인터페이스를 사용하면 기본형을 사용하지 못하는것은 아니지만 박싱 언박싱 과정이 발생하고 이는 메모리를 더 소비하게 되며 메모리를 탐색하는 과정 까지 추가되어 성능에 좋지 않습니다.
그래서 우리는 기본형에 특화된 함수형 인터페이스가 존재하는 것도 알아야 하며 또 그것을 잘 활용 할 수 있어야 합니다.

```java
public interface IntPredicate {
	boolean test(int t);
}

IntPredicate evenNumbers = (int i) -> i % 2 == 0;

```

## 5. 함수형 인터페이스의 예외 처리

함수형 인터페이스는 예외를 던지는 동작을 허용하지 않습니다.
예외를 던지는 람다 표현식을 만들려면 확인된 예외를 선언하는 함수형 인터페이스를 직접 정의 하거나 
람다를 try catch 블록으로 감싸야 합니다.

위의 BufferedReaderProcessor 인터페이스는 직접 정의한 함수형 인터페이스 입니다.

```java
public interface BufferedReaderProcessor {
	String process(BufferedReader b) throws IOException;
}
```

하지만 함수형 인터페이스를 만들기 어려운 상황이면 람다식에 직접 try catch 블록을 작성 해주어야 합니다.
```java
Function<BufferedReader, String> f = (BufferedReader b) -> {
	try{
		return b.reaadLine();
	}
	catch(IOException e){
		throw new RuntimeException(e);
	}
}
```

## 6. 형식 검사 , 추론 , 제약

람다가 사용되는 콘텍스트를 이용해서 람다의 형식을 추론 할 수 있습니다.

먼저 형식 확인 과정을 살펴 봅시다.

```java
List<Apple> heavierThan150g = filter(inventory ,(Apple apple) ->
									apple.getWeight()>150);
```


1. 아하 람다식이 filter 메서드에 사용되고 있구나?!
2. filter 메서드를 확인해보자 !  두번째 인자로`Predicate<Apple> p` 를 기대하고 있구나.
3. Predicate 의 추상메서드는 무엇이지? > boolean test(Apple apple) 이구나!
4. 람다식 이녀석은 Apple 을 인자로 받고 boolean 결과를 내뱉어야 겠군!

람다식은 형식만 맞으면 사용 할 수 있기때문에 다양한 함수형 인터페이스와 호환이 가능합니다.

```java
Comparator<Apple> c1 = 
	(Apple a1, Apple a2) -> a1.getWeigth().compareTo(a2.getWeigth());
ToIntBiFunction<Apple, Apple> c2 = 
	(Apple a1, Apple a2) -> a1.getWeigth().compareTo(a2.getWeigth());
BiFunction<Apple, Apple, Integer> c3 = 
	(Apple a1, Apple a2) -> a1.getWeigth().compareTo(a2.getWeigth());
```

Consumer 나 Predicate 는 void를 반환 하는 형식을 가지고 있습니다.
하지만 아래와 같이 호환이 가능합니다.

```java
Pridicate<String> p = s -> list.add(s);
```

list.add 메서드는 원래 boolean 을 반환하는 메서드 이지만 람다식에서는 호환이 가능 합니다.

>Quiz
>아래의 코드를 컴파일 할수 없는 이유는?

```java
	Object o = () -> {System.out.println("손가락이 너무 아파..");};
```

위의 코드를 컴파일 할 수 있는 코드로 만드는 방법은 2가지가 있습니다.

```java
Runnable r = () -> {System.out.println("손가락이 너무 아파..");};

Object o = (Runnable) () -> {System.out.println("손가락이 너무 아파..");};

```

그리고 두번째 방법은 같은 함수형 디스크립터를 가진 두 함수형 인터페으스를 갖는 메소드를 오버로딩 할때  활용 될 수 있습니다.

```java
@FuctionalInterface

interface Action{
	void act();
}
public void excute(Runnable runnable){
	runnable.run();
}

public void excute(Action<T> action){
	void act();
}

excute( (Action) () -> {} );

```


람다식의 형식을 확인 하는 과정을 이해 했다면 람다식의 파라미터를 보다 간단하게 전달해도 컴파일러는 충분히 파리미터를 추론 할 수 있다는 것을 알 수 있습니다.

```java
Comparator<Apple> c = 
	(Apple a1, Apple a2) -> a1.getWeigth().compareTo(a2.getWeight());
Comparator<Apple> c = 
	(a1, a2) -> a1.getWeigth().compareTo(a2.getWeight());
	
```

## 7. 람다의 지역 변수 사용

람다는 파라미터로 넘겨진 변수 뿐만 아니라 외부에서 정의된 변수도 활용 할 수 있습니다.
이를 람다 캡처링이라고 합니다.
인스턴스 변수와 정적 변수를 모두 사용 할 수 있지만 한가지 제약이 있습니다.
지역 변수는 final 로 선언되거나 final 처럼 동작하고 있어야 합니다.
그렇지 않으면 컴파일 할 수 없습니다.
