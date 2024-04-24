#study #cs-study


앞장에서 스트림의 예시와 설명을 엄청 해주었는데 이번 챕터는
이 장에서는 스트림 API가 지원하는 다양한 연산을 살펴본다.

여기서는 기본적인 문법들을 설명하는게 주요포인트다.

### filter

스트림을 활용할 때 가장 자주 많이 사용할만한 문법 중 하나

흔히 말하는 어떠어떠한 조건으로 필터링한다의 역할을 한다.

**프레디케이트**(불리언을 반환하는 함수) 와 조건등을 활용해 Boolean 값을 기준으로 동작한다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);

numbers.stream()
        .filter(i -> i > 0)
        .forEach(System.out::println);
        
numbers.stream()
        .filter(i -> isNumber(i))
        .forEach(System.out::println);    
```



-----

### distinct

고유한 요소만 반환하는 메소드 또한 있다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
numbers.stream()
        .distinct()
```

중복요소를 제거하고 유일한 값만 주는대

>**이때 대상이 객체일때는 어떻게 할까?**

<span style="opacity: 0.1;"> hashcode와equals가 반드시 구체 선언되어야한다. </span>

-----

JDK 9 이상 부터  사용 가능한 슬라이싱 문법도 있다.

filter는 모든 요소를 순회하며 프레디케이트 함수를  호출한다.
하지만 이런 경우도 있을 수 있다.

- 리스트 중 조건에 맞는 하나의 요소까지만 처리하고 싶은 경우

1. **정렬된 데이터에서 특정 기준을 만족하는 첫 부분만 처리하기:**
   예를 들어, 날짜별로 정렬된 주문 목록이 있고, 특정 날짜 이전의 주문만 처리하고 싶을 때
   `takeWhile`을 사용할 수 있다.
   이 메소드는 지정된 날짜까지의 주문만 고려하고, 그 이후의 주문은 무시하므로 처리 속도가 빨라진다.

```java
List<Order> orders = // 주문 목록
LocalDate cutoffDate = // 특정 날짜

List<Order> filteredOrders = orders.stream()
                                    .takeWhile(order -> order.getDate().isBefore(cutoffDate))
                                    .collect(Collectors.toList());

```

2. **조건이 만족되지 않는 첫 요소까지만 처리하기:**
   어떤 성적 목록이 있고, 처음으로 낙제 점수(예: 50점 미만)가 나올 때까지의
   학생들만을 대상으로 특정 처리를 하고 싶을 때 `takeWhile`을 사용할 수 있다
```java
List<Integer> scores = // 성적 목록
List<Integer> passingScores = scores.stream()
                                    .takeWhile(score -> score >= 50)
                                    .collect(Collectors.toList());
```

3. **특정 조건이 처음으로 만족되는 지점부터의 요소들 처리하기:**
   예를 들어, 온도 데이터가 시간 순으로 정렬되어 있고,
   처음으로 온도가 어떤 기준치(예: 25°C)를 초과하는 지점부터의 데이터만 분석하고 싶을 때
   `dropWhile`을 사용할 수 있다.
```java
List<Double> temperatures = // 온도 목록

List<Double> highTemperatures = temperatures.stream()
                                            .dropWhile(temp -> temp <= 25.0)
                                            .collect(Collectors.toList());
```


- takeWhile 는 프레디케이트가 true 첫번째 요소까지 도달한 동안에만
- dropWhile 는 프레디케이트가 false 가 될 때까지 반복한다,


> **단점을 뭘까?**

<span style="opacity: 0.1;"> 병렬 스트림을 사용 못한다 </span>
<span style="opacity: 0.1;"> 보통의 경우 정렬이 선제조건이다 </span>


-----


### limit

- 접근 중인 요소의 갯수제한을 거는 문법 간단하다.
- .limit(3) : 3개만 조회하겟다.
```java
List<Double> temperatures = // 온도 목록

List<Double> highTemperatures = temperatures.stream()
							.dropWhile(temp -> temp <= 25.0)
							.limit(3)
							.collect(Collectors.toList());
```


-----------
### skip
- 이름처럼 요소를 건너뛴다
- 이때 인자는 정수라 몇개를 건너뛰는 등의 역할만 할 수 있다.

만약 이럴 경우에는 ?
```java
List<Integer> skipList = asList(1,2,3,4,5)

skipList.stream()
		.skip(6)
		.collect(Collectors.toList());
```

<span style="opacity: 0.1;"> 빈 스트림을 반환한다.</span>


--------

### map

- 함수를 인자로 받아 순회중인 각 요소에 적용해 새로운 요소로 매핑한다.

>매핑한다라는 말이 어색할 수 있는데 책에서 잘 설명해준다
>이때 기존값을 변경 하는게 아닌 변환에 가깝기 때문에 매핑이라는 단어를 활용한다 말한다


```java
List<String> disheNames = menu.stream()
						//.map(Dish::getName)
						.map(d-> d.getName())
						.collect(toList());
```

map을 통하면 거의 왠만한 내용을 다 처리할 수 있다
다만 그게 힘든 경우가 있는데!

가장 대표적인 문자열을 하나하나 짜르고 싶다.


```java
input  : ["cat","dog"]
output : [ "c", "a", "t", "d", "o", "g" ]

List<String[]> results = animals.stream()
							.map(animal -> animal.split(""))
							.collect(Collectors.toList());

result : [  [  "c", "a", "t"  ] , [ "d", "o", "g" ]  ]						
```

두개의 배열 요소가 생성되는 문제가 발생한다.
이런 울퉁불퉁한 요소를 평탄화 하는걸 flatMap이라고 한다.

```java
List<String[]> results = animals.stream()
							.map(animal -> animal.split(""))
							.flatMap(animal -> Arrays.stream(animal))
							// .flatMap(Arrays::stream)
							.collect(Collectors.toList());
```




![](https://i.imgur.com/V5xW1HA.png)



-----------------

### Match
#### anyMatch
- 프리디케이트가 적어도 한 요소와 일치하는 지 확인
- 결과값은 Boolean
```java
if (menu.stream().anyMatch(Dish::isVegetarian)) {
  System.out.println("The menu is (somewhat) vegetarian friendly!");
}
```
#### allMatch
- 프리디케이트가 모든 요소와 일치하는 지 확인
- 결과값은 Boolean
```java
boolean isHealthy = menu.stream()
                        .allMatch(dish -> dish.getCalories() < 1000);
```
#### noneMatch
- allMatch의 반대 역할
- 결과값은 Boolean
```java
boolean isHealthy = menu.stream()
                        .noneMatch(dish -> dish.getCalories() >= 1000);
```
### findAny
- 스트림에서 임의의 요소를 반환한다.
- 반환값은 Optional

```java
Optional<Dish> any = menu.stream() 
						.filter(Dish::isVegetarian) 
						.findAny(); 
```

findFirst 또한 있는데 병렬환경에서는 첫번째 요소 찾는게 제약이 따르게 때문에
요소의 반환 순서에 대한 문제가 없다면 병렬환경에서는 findAny를 사용한다.

>**위 메소드들은 어떤 연산자일까**
><span style="opacity: 0.1;"> 스트림이 아닌 값을 반환함으로 최종 연산자.</span>


필터와의 차이점은?
<span style="opacity: 0.1;"> 중간연산자와 최종연산자.</span>

-----------
### reduce

이전까지의 최종 연산은 boolean  / void  / Optional / collections들인대

리듀스 연산을 활용해 스트림 요소를 조합해서 복잡한 질의도 가능하다.
`reduce`는 마치 이전의 `map`과 `filter` 등에 의해 변환된 스트림을
하나의 결과로 종합하는 "결합자"와 같은 역할을 한다.

예시

예를 들어,
주문 리스트에서 특정 조건(예: 특정 가격 이상의 주문)에 맞는 주문들의 총액을 계산하고 싶다고 가정해 보면.
이 경우 `map`, `filter`, `reduce`를 함께 사용할 수 있다.

1. **`filter`**를 사용해 특정 가격 이상의 주문만을 선택합니다.
2. **`map`**을 사용해 각 주문의 가격으로 변환합니다.
3. **`reduce`**를 사용해 모든 선택된 주문의 가격을 합하여 총액을 계산합니다.

```java
List<Order> orders = // 주문 리스트;
double totalPriceOfExpensiveOrders = orders.stream()
    .filter(order -> order.getPrice() > 100) // 가격이 100 이상인 주문 필터링
    .mapToDouble(Order::getPrice) // 각 주문의 가격으로 매핑
    .reduce(0, Double::sum); // 가격들의 총합 계산
    //.reduce(0, (x, y) -> x + y); // 가격들의 총합 계산
```

- 0은 연산의 초기값
    - 만약 초기값이 없다면 결과는 옵셔널로 반환된다.
- x는 누적결과 이전까지의 연산결과
- y는 현재 접근한 값



> 이러한 처리는 종이가 작은 조각이 될때까지 반복한대 표현해 **폴드**로 말하기도 한다.


-----

reduce연산은 새로운 값을 이용해서 스트림의 모든 요소를 소비할 떄 까지 람다를 반복 수행하면서
최댓값을 생산한다. 다음처럼 reduce를 이용해서 스트림의 최댓값을 찾을 수 있다.

```java
Optional<Integer> max = numbers.stream().reduce(Integer::max);

Optional<Integer> min = numbers.stream.reduce(Integer::min);

// Intger::min 대신 (x, y) -> x < y ? x : y를 사용해도 무방하지만 가독성이 더 좋음
```

map 과 reduce를 연결하는 기법을 map-reduce 패턴이라고 하며, 쉽게 병렬화 하는 특징이 있다.

> max() , min() 메소드도 잇는데 무슨 차이일까

- **용도와 표현력**: `reduce`는 보다 일반적인 연산에 사용되며, 사용자가 연산의 방식을 정의해야 한다.
  반면, `max()`와 `min()`은 특정한 용도(최대값과 최소값 찾기)에 최적화된 메소드.
  `max()`와 `min()`을 사용하는 것이 해당 작업에 대해 더 명시적이고 읽기 쉬운 코드를 작성할 수 있게 한다.

- **결과 타입**: `reduce` 연산의 결과는 `Optional<T>`가 아닌 경우 초기값을 제공했을 때의 타입
  초기값 없이 사용할 경우 `Optional<T>`를 반환합니다. `max()`와 `min()`은 항상 `Optional<T>`를 반환한다.
  이는 스트림이 비어 있을 때 최대값이나 최소값이 존재하지 않을 수 있기 때문.


--------------

#### 스트림 연산: 상태 없음과 상태 있음


- map / filter는 입력 스트림에서 데이터를 받아 결과를 출력 스트림으로 보낸다
  그래서 이때 지정한 람다나 익명메소드는 가변적인 상태가 허용되지 않는다.(순수함수만 가능하다.)
- reduce, sum, max 같은 연산은 결과를 누적할 내부 상태가 필요하다.
  스트림이 처리하는 요소 수와 관계없이 내부 상태의 크기는 **한정**(bound)되어 있다.
  이 한정은 할당된 메모리를 의미한다.
- sorted, distinct 같은 메소드는 입력 스트림 출력 스트림 왜에도 이전 값들을 알고 있어야 처리가 가능하다.
  모든 요소가 버퍼에 추가되어야하기 때문에 스트림의  크기가 매우 클 경우 문제가 될수 있다.
  ex) 소수중 가장 큰 수 , 파이의 제일 끝자리


**상태 없는 연산 ** :   순수함수만 받을 수 있는 연산
**상태 있는 연산 ** :  가변적인 값이 있고 순수하게 동작하기 힘든 연사
