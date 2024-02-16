#study #cs-study

이 챕터는 지금 까지 배운 람다, 메서드참조 , 스트림등의 기능을 활용해서

가독성이 좋고 유연한 코드로 리팩터링하는 방법론에 대해 설명하는 챕터이다.
  
----  

> **코드의 가독성이란?**

매우 개별적인 소개이지만 책에서는 이렇게 설명한다.

- 어떤 코드를 다른 개발자가 봐도 이해하기 쉽고 유지보수 하기 쉽게한 것
- 코드의 가독성이 좋다는 것은 코드의 의도를 명확하게 드러내는 것이다.

이런 리팩토링은 세가지 정도 소개한다.

- 익명클래스를 람다로 리팩토링
- 람다를 메서드 참조로 리팩토링
- 명령형 데이터 처리를 스트림으로 리팩토링

----  

### 익명클래스를 람다로 리팩토링

익명클래스는 코드가 길어지고 가독성이 떨어진다. 람다로 리팩토링하면 코드가 간결해지고 가독성이 좋아진다.

```java  
// 익명클래스  
Runnable r = new Runnable() {  
    @Override    public void run() {        System.out.println("Hello");    }};  
  
// 람다  
Runnable r = () -> System.out.println("Hello");  
```  

- 모든 익명 클래스를 람다로 바꿀 수 있는 것은 아니다.
  - 익명 클래스에서는 this 키워드가 익명 클래스 자신을 가리킨다.
  - 람다에서는 바깥 클래스를 가리킨다.
  - 익명 클래스는 감싸고 있는 클래스의 변수를 가릴 수 있다.
  - 람다는 가릴 수 없다.

```java  
int a = 10;  
  
Runnable r1 = () -> {  
    int a = 2; //컴파일 에러 - already define in the scope    System.out.println(a);};  
  
Runnable r2 = new Runnable() {  
    public void run() {        int a = 2;        System.out.println(a);    }};  
```  

- 익명클래스는 인스턴스화 될때 명시적 타입 선언이 정해지는 반면
- 람다는 콘텍스트에 따라 달라지기 때문(타겟타입이 무엇인지에 따라 달라짐) 타입을 명시적으로 선언할 필요가 없다.

> 콘텍스트란 ? -> 람다가 사용되는 위치를 의미한다.

```java  
// 익명클래스  
Comparator<Apple> byWeight = new Comparator<Apple>() {  
    public int compare(Apple a1, Apple a2) {        return a1.getWeight().compareTo(a2.getWeight());    }};  
  
// 람다  
Comparator<Apple> byWeight = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());  
```  

1. 반환 타입을 명시적으로 선언할 필요가 없다. (람다는 컴파일러가 타입을 추론할 수 있기 때문이다.)
2. 람다는 한줄로 작성할 수 있다.(중괄호와 return문을 생략할 수 있다.)

#### 여기서 더 줄이는 방식이 있을까?


<span style="opacity: 0.1;">byWeight = Comparator.comparing(Apple::getWeight);  </span>


-----  


### 람다를 메서드 참조로 리팩토링

메서드 참조는 람다의 간단한 대안이다.

메서드 참조의 메서드명으로 코드의 의도를 명확하게 알릴 수 있기 때문에

람다 표현식 대신 메서드 참조를 이용하면 가독성을 높일 수 있다.

```java  
Function<String, Integer> stringToInteger = (String s) -> Integer.parseInt(s);  
  
FunctionDescriptor<String, Integer> stringToInteger = Integer::parseInt;  
```  

```java  
Map<CaloricLevel, List<Dish>> dishesByCaloricLevel =  
        menu.stream()                .collect(                        groupingBy(dish -> {                            if (dish.getCalories() <= 400) return CaloricLevel.DIET;                            else if (dish.getCalories() <= 700) return CaloricLevel.NORMAL;                            else return CaloricLevel.FAT;                        }));  
```  

이 코드에서는 각각의 요소에 대해 getCalories()를 호출해, 그 기준에 따라 CaloricLevel을 반환한다.

```java  
public class Dish {  
    ...  
    public CaloricLevel getCaloricLevel() {        if (this.getCalories() <= 400) return CaloricLevel.DIEF;        else if (this.getCalories() <= 700) return CaloricLevel.NORMAL;        else return CaloricLevel.FAT;    }}  
  
Map<CaloricLevel, List<Dish>> dishesByCaloricLevel =  
        menu.stream().collect(groupingBy(Dish::getCaloricLevel));  
```  

개선한 부분은 ?

<span style="opacity: 0.1;"> CaloricLevel을 메소드로 추출해 역할을 분리했다. </span>  

  
----  

### 명령형 데이터 처리를 스트림으로 리팩토링

명령형은 보통 우리가 사용하는 방식의 프로그래밍이다.

- for, if, else, switch 등의 제어문을 사용한다.
- 명령형 프로그래밍은 어떻게 할 것인가를 명시하는 방법이다.
- 이 코드를 병렬적으로 처리하는것은 매우 귀찮고 어렵다.

```java  
class main() {  
    public static void main(String[] args) {  
        List<String> dishNames = new ArrayList<>();        for (Dish dish : menu) {            if (dish.getCalories() > 300) {                dishNames.add(dish.getName());            }        }  
    }}  
```  

이런 부분은 스트림으로 리팩토링할 수 있다.

```java  
List<String> dishNames = menu.parallelStream()  
        .filter(dish -> dish.getCalories() > 300)        .map(Dish::getName)        .collect(toList());  
```  

> 하지만 switch , if else 등 여러 제어문을 사용하는 경우에는 스트림으로 리팩토링하기 어렵다.

병렬성은 항상 빠를까?

<span style="opacity: 0.1;">  

- 병렬성은 항상 빠르지 않다.
- 병렬성은 스트림의 크기, 요소의 수, 요소의 처리 시간 등에 따라 달라진다.
- 병렬성은 스트림의 크기가 일정 수준 이상이어야 효과적이다.
- 병렬성은 요소의 처리 시간이 일정 수준 이상이어야 효과적이다.

</span>  
  
-----  

### 코드 유연성 개선

람다 표현식을 이용하려면 함수형 인터페이스를 사용해야 한다.

주로 사용하는 패턴은 다음과 같다.

- 조건부 연기 실행
- 실행 어라운드

#### 조건부 연기 실행

조건부 연기 실행은 코드를 실행하는 시점을 연기하는 패턴이다.

```
if(log.isLoggable(Level.FINER)){  
        log.finer("Problem: "+generateDiagnostic());}```  
  
# 위 코드는 log가 FINER 레벨을 로깅할 수 있는지 확인한 후에 로깅을 수행한다.  
# 이 코드는 불필요한 문자열 연산을 수행한다.  
# **loggin의 상태가 프로덕션 코드로 노출된다**    
```  

```
logger.log(Level.FINER, () -> "Problem: " + generateDiagnostic());  
```

- 람다를 이용하면 로깅 코드를 람다로 감싸서 필요한 로깅 레벨이 활성화되어 있을 때만 람다를 실행할 수 있다.

이 걔념이 진짜 어려웠는데.
쉽게 다시 가보자

조건부 연기 실행은 이름처럼 조건에 따라 코드의 수행을 뒤로 미루는 방식이다.

다만 이 방식을 람다의 형식으로 지원한다는 의미다.

다른 예제를 한번 만들어봤다.

```java
public static String 로그생성(){
    try {
        Thread.sleep(100000); // 100초 기다림 (== 100000ms)
    } catch(Exception ex) { throw new RuntimeException(ex); }
    return "새 로그 메시지";
}

public static void 로그출력기본(final Boolean 플래그, final String 메시지){
    if(플래그 == true)
        System.out.println(메시지);
}

public static void 로그출력지연(final Boolean 플래그, Supplier<String> 메시지공급자){
    if(플래그 == true)
        System.out.println(메시지공급자.get());
}

public static void main(String[] args){
    Boolean 플래그 = true;
    로그출력기본(플래그, 로그생성());
    System.out.println("?"); // 기본과 지연 방식 구분을 위한 구분자
    로그출력지연(플래그, () -> 로그생성());
}

```

- Supplier< T > 를 한번 들어가보자

#### 실행 시 어떻게 될까?

<span style="opacity: 0.1;">  
실행시 메소드 실행 시 평가 전략에 따라서 인자 메소드가 우선적으로 실행된다.
</span>  

[Evaluation strategy](https://velog.io/@kylekim2123/Java%EC%9D%98-Evaluation-strategy%ED%8F%89%EA%B0%80-%EC%A0%84%EB%9E%B5%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC)

----


### 실행 어라운드

메서드 실행 전이나 후에 매번 같은 동작을 반복적으로 수행하는 코드가 있다면 람다로 변환해서 코드 중복을 줄일 수 있다

보일러 플레이트 코드를 없애는 효율적인 방식

- 파일 처리 코드
- 데이터베이스 연결 코드


-----  

## 람다로 객체지향 디자인 패턴 리팩토링하기.

JDK가 확장될 수록 기존 코드 관행이 변하는건 당연한 얘기다.

- JDK5에서 추가된 for-each 문으로 기존의 for문에서 선택지가 늘어났다.
- JDK7에서 추가된 try-with-resources로 기존의 try-finally를 대체할 수 있게 되었다.
- JDK7에서 추가된 <> 다이아몬드 연산자로 기존의 제네릭 타입을 간결하게 표현할 수 있게 되었다.
  - 이때 부터 타입추론이 활기를 띠기 시작했다.
- JDK8에서 추가된 람다로 기존의 익명클래스를 대체할 수 있게 되었다.


이렇게 많이 변화한 다양한 패턴을 유형별로 나눈게 디자인 패턴이다.

람다를 활용해서 이런 디자인 패턴을 더 편하게 활용하는 방식을 소개하는 챕터다.

- 전략(strategy)
- 탬플릿메소드(template method)
- 옵저버(observer)
- 의무 체인(chain of responseibility)
- 팩토리(factory)

### 전략(strategy)

> 실행 시점에 전략적으로 원하는 알고리즘을 지정하는 방식
>
> ![](https://i.imgur.com/go0zyQ4.png)

-> conduit task calculrater

책의 예시도 아주 적절하게 잘 설명되어있다.

```java
@FunctionInterface
public interface ValidationStrategy {
    boolean validate(String s);
}

public class MyValidator {
    private final ValidationStrategy validationStrategy;
    
    public MyValidator(final ValidationStrategy validationStrategy) {
        this.validationStrategy = validationStrategy;
    }
    
    public boolean validate(String s){
        return validationStrategy.validate(s);
    }
}

==========================================================================================

public class IsNumber implements ValidationStrategy {
    @Override
    public boolean validate(final String s) {
        return s.matches("\\d+");
    }
}
public class IsOverFiveLength implements ValidationStrategy {
    @Override
    public boolean validate(final String s) {
        return s.length() > 5;
    }
}
==========================================================================================

public class Main {
    public static void main(String[] args) {
        String input = "123456";
        
        MyValidator numberValidator = new MyValidator(new IsNumber());
        System.out.println(numberValidator.validate(input));
        
        MyValidator fiveLengthValidator = new MyValidator(new IsOverFiveLength());
        System.out.println(fiveLengthValidator.validate(input));
    }
}
```

간략하고 잘 정리된 느낌인대.
전략패턴의 의미를 다시 생각해보자
왜 잘된 패턴일까?

<span style="opacity: 0.1;">  전략적으로 검증 조건 알고리즘을 변경한다.  </span>  

이걸 좀더 편하고 직관적으로 람다 표현식으로 사용하면

이런 느낌이 된다.

```java
public class Main {
    public static void main(String[] args) {
        String final input = "123456";
        
        Validator IsNumber         = new Valieator( input -> input.matches("\\d+"));
        Validator IsOverFiveLength = new Valieator( input -> input.length() > 5  );
    }
}
```


### 탬플릿메소드(template method)

> 전략 패턴과 유사하지만 다른 패턴 모든 알고리즘이 아닌 알고리즘의 일부분만 바꾸는 경우

```java
abstract class OnlineBanking {
   public void processCustomer(int id) {
      Customer c = Database.getCustomerWithId(id);
      makeCustomerHappy(c);
   }
   abstract void makeCustomerHappy(Customer c);
}
```

사용자의 아이디를 입력받아 고객 정보를 가져오고 서비스를 제공하는 추상 클래스

이때 사용자에게 어떻게 좋은 서비스를 제공하는지는 템플릿에 따라 달라진다.

어떤 사용자는 빠른 처리가 좋은 서비스이고 , 어떤 사용자는 정확하고 친철한 설명이 좋은 서비스이다.

람다표현식을 사용해 좀더 좋은 방식으로 리팩토링 가능핟.

```java
new OnlineBankingLambda().processCustomer(1337,(Custommer c) -> SYS.OUT("FAST"))
```

위의 코드와 동일하지만 매우 짦아졌다.


### 옵저버(observer)

옵저버도 이야기 했던 디자인패턴인대.

Subject 라는 주체에서 Observer들에게 알림을 보내는 형태의 디자인 이였다.

그때도 이야기 했지만 GUI 환경 , 몇몇 특수한 환경에서만 처리되기 때문에 .

코드를 첨부한 깊은 설명은 생략하겟다.

책에서도 복잡성에 따라 그냥 기존 클래스를 쓰라고 권고한다.
![](https://i.imgur.com/muEkZQj.png)


### 의무 체인(chain of responseibility)

의무 체인이라는 패턴은 처음 들을수도 있는데.

하나의 객체가 다른 객체로 결과를 전달하고 또 다른 객체로 전달하는 체이닝 기법

이 동작만 봐도 감이 올꺼 같은데 우리가 최근에 가장 많이 얘기하는 의무체인 기법의 예시가 뭐가 있을까??

<span style="opacity: 0.1;">  스트림의 체이닝  </span>  

### 팩토리(factory)

인스턴스 로직을 노출시키지 않고 객체를 생성할때 주로 사용된다.

다만 이 내용같은 경우 가능하다~지 적용하라가 아니라 코드는 생략

구현의 문제가 많기 때문에 권고되지 않는 방식이고 인스턴스의 로직을 노출시키지 않는건

다른 기법으로 더 편하게 구현 가능하다.


-------------------


### 람다 테스팅

람다는 적응한다면 작성하기도 쉽고 빠르게 개발할 수 있다..

그만큼 테스트도 중요하다.

- 람다의 목표는 정해진 동작을 다른 메서드에서 사용할 수 있도록 하나의 조각으로 캡슐화 하는것이다.
- 람다 표현식을 사용하는 메서드의 동작을 테스트 함으로서 람다 표현식을 검증 할 수 있다.
- 복잡한 로직이 포함된 람다를 구현하게 된다면 로직을 분리 하거나 메서드 레퍼런스를 활용하도록 하자.
- 함수를 인수로 받거나 다른 함수를 반환하는 메서드는 사용하기도, 테스트하기도 어렵다.

다른 내용도 많지만 한줄로 요약이 가능하다.

람다를 테스트하는건 동작에 집중해라 그뿐.

-----


### 디버깅

디버깅도 사실 말이 많은데 에러 발생 시 어느 포인트를 봐라 같은  이야기를 소개한다.

람다 표현식은 이름이 없는 클래스다 보니 컴파일러가 컴파일 시점에 랜덤한 이름을 부여하단다.

![](https://i.imgur.com/sf2Hmc9.png)


