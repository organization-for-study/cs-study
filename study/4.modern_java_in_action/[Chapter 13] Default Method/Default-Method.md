
### 1. 디폴트 메서드란?

> 디폴트 메서드란? 
> 디폴트 메서드는 기존의 인터페이스에서 작성되는 메서드와는 달리 구현이 포함된 메서드
> 참고 : 디폴트 메서드는 암시적으로 public 입니다.

Java 8에서 인터페이스에 디폴트 메서드가 추가되면서 인터페이스의 기능을 확장 시키고 싶을때 구현체의 내용을 변경하지 않고 확장 시킬 수 있게 되었습니다. 그리고 기존의 추상 메서드에 기능을 덧붙여 사용 할 수 있게 되었습니다.

Java8 부터는 인터페이스에서 사용할 수 있는 static 메서드도 추가 되었습니다.
static 메서드는 클래스의 static 메서드 처럼 사용되기 보다는 주로 helper 메서드로 사용 됩니다.
마찬가지로 static 메서드는 구현 클래스에서 따로 구현하지 않아도 됩니다.

```java
public interface Test {

static String staticMethod() {

return "hello ":

}

default String defaultMethod (String param) {

return staticMethod( ) + param;

}

//Test 인터페이스 구현한 TestImol

public class TestImpl implements Test {

@Override

public String defaultMethod (String param) {

return Test.super.defaultMethod (param) ;

}

public static void main (String[] args) {

TestImpl testImpl = new TestImplI);

System.out.printin(testImpl.defaultMethod("world"'));
}
										  
}
```

> 디폴트 메서드는 인터페이스를 구현하는 모든 구현체에서의 동작을 보장하지 않습니다. 따라서 
> 자바doc(@impleSpec)을 통하여 구현 개발자에게 올바른 동작을 위한 가이드를 제공해야 합니다.


바이너리 호환성, 소스 호환성 , 동작 호환성
- [바이너리 호환성](https://velog.io/@kms8571/JAVA-%EB%B0%94%EC%9D%B4%EB%84%88%EB%A6%AC-%ED%98%B8%ED%99%98%EC%84%B1-%EA%B4%80%EB%A0%A8-%EC%9D%B4%EC%8A%88) - 뭔가를 바꾼 이후에도 에러 없이 기존 바이너리가 실행 될 수 있는 상황
- 소스 호환성 - 컴파일 가능
- 동작 호환성 - 코드를 바꾼 다음에도 같은 입력값이 주어지면 프로그램이 같은 동작을 하는것

>Quiz
>추상 클래스 VS 인터페이스


### 2.디폴트 메서드 활용 패턴

-  불필요하게 많은 "빈" 구현을 효과적으로 제거 할 수 있습니다.
```java
interface Iterator<T> {

boolean hasNext ();

T next ();

default void remove() {

throw new UnsupportedOperationException ();
}
}
```

- 다중 동작 상속 - 인터페이스를 기능별로 분리하여 최소화
```java
public interface Vacuumcleaner {// 진공 청소기

default void vacuumCleaning() {

//진공 청소 구현

}

}

public interface Beddingcleaner {// 침구 청소기

default void beddingCleaning() {

//친구 청소 구현

}

}

public interface WaterMop {// 물 걸레

default void mopping() {

//물 걸레 청소 구현

}

public class SamsungCleaner implements VacuumCleaner, BeddingCleaner, WaterMop {

}
```

### 3. 다중 상속 해결

> 문제 1
```java
public interface A {
  default void run() {
    System.out.println("A : run!");
  }
}
public interface B {
  default void run() {
    System.out.println("B : run!");
  }
}
public class C implements B, A {
  @Override public void run() {
    B.super.run();
}
}
```

>문제 2
```java
public interface A {
default void run() {
  System.out.println("A : run!");
  }
}
public interface B extends A {
default void run() {
System.out.println("B : run!");
  }
}
public class C implements B, A { }
 new C().run(); // B : run!

```

>문제 3
```java
public interface A {
  default void run() {
    System.out.println("A : run!");
  }
}
public interface B extends A {
  default void run() {
    System.out.println("B : run!");
  }
}
public class C implements A, B {
  public void run() {
    System.out.println("C : run!"); }
}
new C().run(); // C : run!
```

private default method 사용 예
[인터페이스에서 default , static 메서드](https://soft-dino.tistory.com/25)
[디폴트 메서드 jpa에서의 활용](https://dkswnkk.tistory.com/730)



