
이번 챕터를 공부하면 최종 연산인 collect 연산을 다양한 곳에 활용 할 수 있습니다.

collect 에 우리가 원하는 동작을 파라미터화 하여 collect의 동작을 유연하게 변경 할 수 있습니다.
그리고 Collectors 유틸리티 클래스는 정적 팩토리 메서드를 통해 자주 사용하는 컬렉터 인스턴스를 제공 합니다.

```java
List<Transaction> transaction = 
	transactionStream.collect(Collectors.toList());
```


첫번째 알아볼 내용은 리듀싱과 요약 입니다.

## 1. 리듀싱과 요약

스트릠에 있는 객체의 숫자 필드의 합계나 평균 등을 반환하는 연산에 리듀싱이 자주 사용됩니다.
이러한 연산을 요약 연산이라고 합니다.
여러 요약 연산을 살펴봅시다.

### 1-1. 스트림 값에서 최댓값 검색

```java
Comparator<Dish> dishCaloriesComparator =
	Comparator.comparingInt(Dish::getCalories);

Optional<Dish> mostCalorieDish =
	menu.stream()
	.collect(maxBy(dishCaloriesComparaor));
```

1. Collectors 클래스가 제공하는 메서드 maxBy는 Comparator의 구현체를 인수로 받습니다.
2. menu 스트림이 비어있을 수도 있기 때문에 Optional로 결과를 받습니다.

### 1-2. 합계 평균 등의 연산

Collectors 클래스는 summingInt 팩토리 메서드를 제공합니다.
아래의 예제를 살펴보면 쉽게 이해가 될것입니다.

```java
int totalCalories = menu.stream.collect(summingInt(Dish::getCalories));
double avgCalories = menu.stream.collect(averagingInt(Dish::getCalories));
```

### 1-3. 문자열 연결

joining 메서드는 내부적으로 StringBuilder를 이용해서 문자열을 하나로 만듭니다.

```java
String shortMenu = menu.stream().map(Dish::getName).collect(joining(", "));
```

### 1-4. reducing 메서드

지금까지 살펴본 모든 컬렉터는 reducing 팩토리 메서드로도 정의 할 수 있습니다.
즉 범용 Collectors.reducing 으로 구현할 수 있습니다.

```java
int totalCalories = menu.stream()
			.collect(reducing(0,Dish::getCalories,(i,j)-> i + j));
```

reducing 은 세 개의 인수를 받습니다.
첫번째 인수는 리듀싱 연산의 시작 값이거나 스트림에 인수가 없을 때는 반환값 입니다.
두번째 인수는 변환 함수입니다.
세번째 인수는 같은 종류의 두 항목을 하나의 값으로 연산하는 BinaryOperator 입니다.


```java
Optional<Dish> mostCalorieDish =
		menu.stream().collect(reducing(
		(d1,d2) -> d1.getCalories() > d2.getCalrories() ? d1 : d2));
```

한개의 인수를 갖는 reducing 팩토리 메서드는 스트림의 첫번째 요소를 첫번째 인수로 받으며,
변환 함수에는 자신을 그대로 반환하는 항등함수를 사용합니다.

>Quiz 다음 중 잘못된 코드는?

```java
String shortMenu = menu.stream().map(Dish::getName)
					.collect(reducing( (s1,s2)-> s1 + s2) ).get();

String shortMenu = menu.stream()
			.collect( reducing((d1,d2)-> d1.getName() + d2.getName() ).get();
String shortMenu = menu.stream()
			.collect( reducing( "",Dish::getName,(s1,s2)-> s1 + s2));
```

## 2. 그룹화

### 2-1. 그룹화 맛보기

```java
Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap<>();  
for (Transaction transaction : transactions) {  
	Currency currency = transaction.getCurrency();  
	List<Transaction> transactionsForCurrency =
				transactionsByCurrencies.get(currency);  
	if (transactionsForCurrency == null) {  
		transactionsForCurrency = new ArrayList<>();  
		transactionsByCurrencies.put(currency, transactionsForCurrency);  
	}  
	transactionsForCurrency.add(transaction);  
}
```

위의 코드는 스트림을 활용하지 않고 통화별로 트랙잭션 리스트를 그룹화 합니다.
스트림 및 람다를 활용하면 위의 코드보다 조금 더 직관적이고 한눈에 파악하기 쉽게 만들 수 있습니다.

Collectors 에서 제공하는 groupingBy() 메서드를 통해 위의 예제를 간단한 코드로 바꾸어 보겠습니다.

```java
Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap<>();
transcations.stream().collect(groupingBy(Transcation::getCurrency));
```

groupingBy() 메서드는 분류함수를 인수로 받습니다. 분류함수가 반환하는 값을 키로 하고 각 키에 대응하는 스트림의 모든 항목  리스트를 값으로 갖는 맵이 반환 됩니다.

### 2.2 두가지 기준으로 그룹화 하기

요리의 종류와 칼로리를 기준으로 그룹화 해보겠습니다.

```java
Map<Dish.Type, List<Dish>> caloricDishesByType =
		menu.stream().filter(dish -> dish.getCalories() > 500)
					 .collect(groupingBy(Dish.getType));
```

>Quiz 위의 코드에서 발생 할 수 있는 문제점은 무엇 일까요?

```java
Map<Dish.Type, List<Dish>> caloricDishesByType =
		menu.stream().collect(groupingBy(Dish.getType,
				filtering(dish->dish.getCalories() > 500,toList())));
```

groupingBy 메서드는 분류 함수만 인수로 받을 수 도 있고, 위의 예저처럼 분류함수와 두번째 인수로 컬렉터를 받을 수 있습니다. 
하나의 인수만을 받는 groupingBy 메서드는 사실 (분류함수,toList()) 의 축약형 입니다.

### 2-3. 다수준 그룹화

```java
Map<Dish.Type, Map<CaloricLevel, List<Dish>> groupDishedByTypeAndCaloricLevel =
menu.stream().collect(  
	groupingBy(dish -> {  
			if (dish.getCalories() <= 400) {  
					return CaloricLevel.DIET;  
			}  
			else if (dish.getCalories() <= 700) {  
			return CaloricLevel.NORMAL;  
			}  
			else {  
			return CaloricLevel.FAT;  
			}  
			}));
```

### 2-4 그룹화 + 데이터 수집

```java
private static Map<Dish.Type, Long> countDishesInGroups() {  
return menu.stream().collect(groupingBy(Dish::getType, counting()));  
}

private static Map<Dish.Type, Integer> sumCaloriesByType() {  
return menu.stream().collect(groupingBy(Dish::getType, summingInt(Dish::getCalories)));  
}

private static Map<Dish.Type, Optional<Dish>> mostCaloricDishesByType() {  
return menu.stream().collect(groupingBy(Dish::getType,  
				reducing((Dish d1, Dish d2) -> d1.getCalories() > d2.getCalories() ? d1 : d2)));  
}
```

>Quiz 3번째 메서드에서 Optional 래퍼를 사용하지 않으면 어떻게 될까요?


```java
private static Map<Dish.Type, Dish> mostCaloricDishesByTypeWithoutOprionals() {  
return menu.stream().collect(  
					groupingBy(Dish::getType,  
						collectingAndThen(  
							reducing((d1, d2) -> 
							d1.getCalories() > d2.getCalories() ? d1 : d2), 
								Optional::get)));  
}
```

Optonal::get 으로 Optional 에 포함된 값을 추출합니다.
위의 퀴즈에서 알아본것 처럼  Optional.empty() 를 반환 할 일은 없으므로 안전한 코드입니다.

### 2-5. 분할

분할 함수라 불리는 프리디케이트를 분류 함수로 사용하여 두개의 그룹으로 분할할 수 있습니다.
문장 그대로 분할 또한 그룹화의 다양한 방식중 하나입니다.

```java
private static Map<Boolean, List<Dish>> partitionByVegeterian() {  
return menu.stream()
		   .collect(partitioningBy(Dish::isVegetarian));  
}

private static Map<Boolean, Map<Dish.Type, List<Dish>>> vegetarianDishesByType() {  
return menu.stream().collect(partitioningBy(Dish::isVegetarian,groupingBy(Dish::getType)));  
}
```


## 3. Collector 인터페이스

### 3-1. 간단한 ToList 컬렉터 구현해보기

```java
public interface Collector<T, A, R>{
	Supplier<A> supplier();
	BiConsumer<A, T> accumulator();
	Function<A, R> finisher();
	BinaryOperator<A> combiner();
	Set<Characteristics> characterlistics();
}
```

1. T는 수집될 스트림 항목의 제네릭 형식 입니다.
2. A는 누적자, 즉 수집 과정에서 중간 결과를 누적하는 객체의 형식입니다.
3. R은 수집 연산 결과 객체의 형식 입니다.(항상 그런것은 아닙니다.)

```java
public class ToListCollector<T> implements Collector<T, List<T>, List<T> >{
	public Supplier<List<T>> supplier() {
		return () -> new ArrayList<T>();
	}
	public BiConsumer<List<T> ,T> accumulator() {
		return (list,item) -> list.add(item);
	}
	public Function<List<T>, List<T>> finisher() {
		return Function.identity();
	}
	public BinaryOperator<List<T>> combiner() {
		return (list1,list2) -> {
			list1.addAll(list2);
			retrun list1;
		}
	}
	public Set<Characteristics> characteristics() {
		return Collections.unmodifiableSet(EnumSet.of(
							IDENTITY_FINISH, CONCURRENT));
	}
	
	
}
```




