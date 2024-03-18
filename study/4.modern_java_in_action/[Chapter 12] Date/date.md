
![](https://i.imgur.com/T0JVTuJ.png)

날짜..... 생각보다 개발하면서 많이 사용하는 값이다.

![](https://i.imgur.com/DGhsSK8.png)


![](https://i.imgur.com/qj6u5Em.png)


원래 어느걸 사용 했나 ?


**java.util.Date** 의 Date 는 늘 문제가 되던 요소였다.
JDK 1 부터 시작된 가장 원시적인 날씨 값을 다루는 요소로서 동작을 했지만.

JDK 1.1에서 **java.util.Calendar**를 대안으로 사용하기도 했지만.

이 역시 문제가 많은 클래스이였다. 

DateFormat은 또 Date클래스에만 적용되기도 하고 Date 와 Calendar 을 혼동해 사용하기도 해 아주 혼돈이였다.

DateFormat 또한 Threed Safe 하지 않은 클래스이기 때문에 문제가 많았다.

이런 날짜 클래스의 문제가 많았기 때문에 별도의 라이브러리를 활용하는 등 예전에는 고생이 많았다.

이때 사용하는 라이브러리가 Joda의 Time 서드파티였는데 . 

JDK 8에서는 이 서드파티를 **java.time** 패키지로 추가 되었다.

![](https://i.imgur.com/pLtdGx5.png)


---------

### 뭐가 문제였나?

Date class의 경우 JDK 1.0 부터 시작된 나름 태초의 라이브러리다.
나름 당연한 말이겠지만 나름 계획하고 설계했음에도 문제점이 많이 발생했다.

#### Date 

1. **변경 가능성(Mutability)**: `Date` 객체는 변경 가능해서, 만들어진 다음에도 내부 상태를 바꿀 수 있어. 
   이건 객체를 변경하지 않길 원할 때 부작용 일으킬 수 있고, **특히 멀티스레드 환경에서는 더 위험하다.**
2. **시간대 처리 부족**: `java.util.Date`는 시간대 정보가 없어. UTC 기준 밀리초로 날짜와 시간 표현하는데, 
   이 때문에 시간대를 제대로 다루기 어렵고 국제화 애플리케이션 만들 때 문제가 된다.   
3. **날짜 시간 API 부족**: `Date`에는 날짜랑 시간 다루는 데 필요한 API가 충분히 없어. 
   예를 들어, 달이나 일, 요일 직접 조작할 메소드가 없어서 간단한 연산도 힘들게 만든다.
4. **디자인 이슈**: `Date`의 설계는 혼란을 준다. `getMonth()` 같은 메소드는 0부터 시작해서 1월이 0, 12월이 11이 되는 방식.
5. **쓰레드 안전성 부족**: `Date`는 스레드 안전하지 않다. 여러 스레드가 동시에 같은 `Date` 객체를 바꾸려고 하면 예상 못한 결과나 데이터 손상이 일어날 가능성이 높다.


### Calender

1. **변경 가능성(Mutability)**: `Calendar` 객체도 변경 가능해서, 객체의 상태를 변경할 수 있다는 점이 문제가 될 수 있다
2. **시간대 처리**: `Calendar`는 시간대를 다룰 수 있긴 하지만, 이를 처리하는 방식이 직관적이지 않고 혼란을 줄 수 있다. 시간대 관리가 중요한 애플리케이션을 개발할 때, 이 부분이 개발을 복잡하게 만들 수 있다.
4. **성능 문제**: `Calendar` 인스턴스는 생성 비용이 상대적으로 높다. 특히, 애플리케이션이 날짜와 시간을 자주 처리해야 하는 경우 성능에 부정적인 영향을 줄 수 있다.
5. **일관성 없는 월 표시**: `Calendar`에서도 `Date` 클래스와 마찬가지로 월을 0부터 시작해. 1월이 0이고, 12월이 11이 되는 식이야. 이런 점은 혼란을 유발하고, 코드를 읽는 사람에게 직관적이지 않다.
6. **사용상 혼란**: `Calendar`와 `Date`를 혼용해서 사용하는 경우가 많은데, 이 둘 사이의 변환 과정에서 혼란이 발생할 수 있다. 또한, `Calendar` 클래스는 여러 상속된 클래스가 있고, 특정 기능을 사용하려면 특정 타입으로 캐스팅해야 하는 경우도 있는데, 이 과정에서 코드의 복잡성이 증가한다.


---------------------



## LocalDate, LocalTime, Instant, Duration, Period 클래스


LocalDate LocalTime은 기본적으로 인스턴스 내에서 불변한 값으로 만들어진다.

```java
LocalDate date = LocalDate.of(2020, 12, 22); // of
int year = date.getYear();
Month month = date.getMonth();
int day = date.getDayOfMonth();
LocalDate now = LocalDate.now(); // 현재 날짜 정보
```

![](https://i.imgur.com/49q3Ayx.png)
![](https://i.imgur.com/ahKP004.png)


정적팩토리 메소드는 입력받은 인자를 받아 시간 값으로 제공해준다.
#### 그렇다면 now() 는?

![](https://i.imgur.com/oKoj3si.png)

![](https://i.imgur.com/bhEyoP1.png)


기본적으로 우리가 주로 사용하는 now()는 현재 사용중인 시스템의 기본 시간대를 추출해 Clock 객체를 만드는 역할이다.

Clock은 기본적으로 시스템 값을 따라가기 때문에

한국에서 작업을 하더라도 내 로컬 개발 환경의 시간대를 유럽같은 시간대로 변경 시 유럽의 now() 시간을 확인한다.

```java
LocalDate localDate = LocalDate.now(Clock.system(TimeZone.getTimeZone("America/Los_Angeles").toZoneId()));
```


--------

>멀티 스레드 환경에서 변경 가능한(mutable) 객체를 공유할 때 발생하는 위험은 
주로 데이터의 일관성과 안정성을 유지하기 어렵다는 데 있다. 
변경 가능한 객체가 여러 스레드에 의해 동시에 접근 및 수정될 경우, 
예측하지 못한 결과나 데이터 손상이 발생할 수 있다. 
이를 "경쟁 조건(race condition)"이라고 부르며, 이는 멀티 스레드 프로그래밍에서 흔히 발생하는 문제 중 하나.

>이러한 문제를 해결하기 위한 방법 중 하나는 불변 객체(immutable objects)를 사용하는 것이다. 
객체가 한 번 생성되면 그 상태를 변경할 수 없기 때문에, 
여러 스레드가 동시에 해당 객체를 참조하더라도 객체의 상태가 변경될 위험이 없습니다. 
예를 들어, `java.time.LocalDate`와 같은 불변 클래스를 사용하면, 
날짜 관련 데이터를 안전하게 공유하고 처리할 수 있다.

----

### LocalDate와 LocalTime

LocalDate 인스턴스는 시간을 제외한 날짜를 표현하는 불변 객체다. 

LocalDate 객체는 어떤 시간대 정보도 포함하지 않는다.

이때 LocalDate가 제공하는 get 메서드에 TemporalField를 전달해서 정보를 얻는 방법도 있다. 

TemporalField는 시간 관련 객체에서 어떤 필드의 값에 접근할지 정의하는 인터페이스다.

```java
int year = date.get(ChronoField.YEAR);
```

ChronoField는 TemporalField의 구현체이며 ChronoField의 열거자 요소를 이용해서 원하는 정보를 얻을 수 있다.

date가 날짜를 표현하는객체라면 Time도있다.

```java
LocalTime time = LocalTime.of(13, 45, 20); // 13:45:20
int hour = time.getHour();
int minute = time.getMinute();
int second = time.getSecond();
```


당연한 말이지만 날짜  + 시간을 합친 ```LocalDateTime``` 도 있다.


--------

### Instant 클래스 : 기계의 날짜와 시간

우리가 생각하는 시간 관념과는 조금 다른 기계관점의 시간 정보도 활용할 일이 생기고는 한다.

유닉스 에포크 시간(Unix epoch time) (1970년 1월 1일 0시 0분 0초 UTC)을 

기준으로 특정 시점 까지의 시간을 초로 표현할 수 있다.


주로 활용되는 예시를 GPT한태 확인해봤다.

![](https://i.imgur.com/NKBKikd.png)


외에도 나는 개인적으로 twitter snowflake 기법 활용할 때 한번 사용한적 있다.


-----------


### Duration과 Period 정의

이 클래스들은 Temporal 인터페이스를 구현하는데, 

Temporal 인터페이스는 특정 시간을 모델링하는 객체의 값을 어떻게 읽고 조작할지 정의하는 인터페이스다.

![](https://i.imgur.com/KhWCJJR.png)


여기서 Duration은 초,나노초로 시간 단위로 표현해서 년월일등의 시간값이 필요하면 Period를 사용해야한다.

--------


### TemporalAdjusters

간단한 날짜 기능이 아닌 더 복잡한 날짜 조정기능이 필요할 때 

with 메서드에 TemporalAdjuster를 전달하는 방법으로 문제를 해결할 수 있다. 

날짜와 시간 API는 다양한 상황에서 사용할 수 있도록 다양한 TemporalAdjuster를 제공한다.

책에서는 상세하게 설명하는데 설명 스킵 왜냐하면 기본적으로 제공하는 함수로 해결 못하는 경우가 거의 없기 때문.


하지만 별도로 쓸일이 있기도 한대 배치잡등 스케줄링이 필요하다면 고려해볼수도 있을거같다.

![](https://i.imgur.com/NXdrPih.png)


----------



### 최종 표현식

![](https://i.imgur.com/CdDMprk.png)


보기 좋은 시간대 표현식 표




### Java 날짜 및 시간 처리의 진화

#### Date와 Calendar의 문제점
- **변경 가능성(Mutability):** 객체가 변경 가능하여 데이터 일관성 유지 어려움, 멀티 스레드 환경에서 위험.
- **시간대 처리 부족:** `Date`는 시간대 정보 없음, `Calendar`의 시간대 처리 복잡.
- **날짜 시간 API 부족:** 충분한 날짜 및 시간 조작 API 제공하지 않음.
- **디자인 및 사용상 혼란:** 월이 0부터 시작하는 등 직관적이지 않은 설계.

#### java.time 패키지의 개선점
- **불변성(Immutability):** 객체의 상태가 변경 불가능하여 데이터의 안정성 보장.
- **명확한 시간대 처리:** `ZonedDateTime`, `OffsetDateTime` 등으로 국제화 애플리케이션 개발 용이.
- **풍부한 API:** 날짜 간의 차이(`Period`, `Duration`), 조건에 따른 날짜 조정(`TemporalAdjusters`) 등 다양한 기능.
- **직관적이고 일관된 API:** 사용자 친화적이며 일관된 API 설계.
- **확장성과 유연성:** 사용자 정의 날짜 시간 클래스 생성 용이, 다양한 캘린더 시스템 지원.

### 결론
`java.time` 패키지는 `Date`와 `Calendar`의 한계를 극복하고, 현대적인 날짜 및 시간 처리를 위한 강력하고 유연한 도구를 제공합니다.

----------
