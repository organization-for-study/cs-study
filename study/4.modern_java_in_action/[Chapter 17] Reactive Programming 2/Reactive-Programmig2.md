

### 1. 리액티브 매니패스토

#### 1-1. 어플리케이션 수준 매니패스토

>  매니페스토 :  어떤 집단의 가치와 목표를 대내외에 선포하고, 행동 변화를 촉구하는 선언문

-  주요기능 : 이벤트 스트림을 블록하지 않고 비동기로 처리
-  별도로 지정된 스레드 풀에서 블록 동작 실행 : 메인풀의 모든 스레드는 방해받지 않고 실행되므로 최적의 상황에서 동작 할 수 있음
-  데이터 베이스나 파일 시스템 접근 등 예측하기 힘든 동작이 블록 동작에 속한다.

<img width="895" alt="2024-04-13_03-24-17" src="https://github.com/organization-for-study/cs-study/assets/140944695/bc78ac80-6c69-417c-b2f1-70e7ae42114b">


#### 1-2. 시스템 수준 리액티브

- 하나의 어플리케이션이 장애를 일으켜도 회복 할 수 있도록 돕는 아키텍처
- 어플리케이션 수준과의 차이점은 `메시지` 주도 라는 점
- 하나의 어플리케이션에서 장애가 발생하면 에러전파를 방지하고 에러를 메시지로 치환하여 다른 컴포넌트(어플리케이션) 으로 보내는 감독자 역할

#### 2. 리액티브 스트림 과 플로 API

-  역압력 : 제공되는 스트림보다 처리되는 스트림이 느릴때 문제가 발생하지 않게끔 하는 장치

#### 2-1. java.util.concurrent.Flow

##### 네개의 인터페이스

- Publisher : 항목을 발행 , 한개의 추상 메서드를 가짐, 수많은 스트림을 제공 할 수 있지만 역압력에 의해 제공 속도 제한
- Subscriber : Publisher 가 발행한 이벤티 리스너로 자신을 등록 
- Subscription : 위의 둘 사이의 제어 흐름 즉 역압력을 관리

```java
@FunctionalInterface
public Interface Publiser<T> {
	void subscribe(Subscriber<? super T> s);
}
@FunctionalInterface
public Interface Subsciber<T> {
	void onSubscribe(Subscription s);
	void onNext(T t);
	void onError(Throwable t);
	void onComplete();
} 
```

-  호출 순서 : OnSubscribe  > onNext* > (onError | onComplete) ?
-  Publisher가 자신을 등록 할 때 onSubscribe 메서드를 호출하고 Subscription 객체를 전달한다.

```java
public interface Subscription {
	void request(long n);
	void cancel();
}
```

-  request : Publisher 에게 주어진 개수의 이벤트를 처리 할 수 있음을 알린다.
-  cancel : 이벤트를 받지 않음을 Publisher 에게 통보한다
- Publisher 와 Subscriber 는 정확하게 Subscription을 공유해야한다.

```java
public interface Processor<T,R> extends Subscriber<T>, Publisher<R> {}
```

- Processor 는 에러를 수신하면 onError 신호를 모든 Subscriber에 에러를 전파한다.


### 3. 정리

- 간단히 말해서 리액티브 프로그래밍은 **생성자가 소비자를 압도하지 못하게 하는 목적**
- 동시성 != 많은 쓰레드
- CPU 충분 But 쓰레드 모자람 -> 처리율 저하 -> 쓰레드를 늘리면 -> CPU 부하로 인한 성능 저하


**스프링 리액티브를 쓰지 말아야하는 이유**
- 스프링 개발자가 아니라면
- 스프링 MVC로 개발해서 별 문제 없이 잘 돌아간다면
- 블로킹 IO 작업이 있다면
- 블로킹이 뭔지는 모르겠으나 **JPA, JDBC, MyBatis**를 쓰고 있다면
- 리모트 서비스, API 호출이 전혀 없고, NoSQL도 사용하지 않고, 메세징 서비스 등을 사용하지 않는다면
- 개발팀이 크고 새롭고 도전적인 기술 학습과 시행 착오에 대한 **부담이 있다면**

**스프링 리액티브 개발을 시작할때 기억할 것**

- 비동기-논블로킹 서비스를 만드는 것이 목적
- Publisher, Subscriber를 직접 만드는 경우는 거의 없다
- `` `scribe()` ``는 스프링 MVC, WebFlux가 담당
- 비동기 – 논블로킹 Publihs 생성은 Reactor 라이브러리가 담당



#### 1-2. 시스템 수준 리액티브

- 하나의 어플리케이션이 장애를 일으켜도 회복 할 수 있도록 돕는 아키텍처
- 어플리케이션 수준과의 차이점은 `메시지` 주도 라는 점
- 하나의 어플리케이션에서 장애가 발생하면 에러전파를 방지하고 에러를 메시지로 치환하여 다른 컴포넌트(어플리케이션) 으로 보내는 감독자 역할

#### 2. 리액티브 스트림 과 플로 API

-  역압력 : 제공되는 스트림보다 처리되는 스트림이 느릴때 문제가 발생하지 않게끔 하는 장치

#### 2-1. java.util.concurrent.Flow

##### 네개의 인터페이스

- Publisher : 항목을 발행 , 한개의 추상 메서드를 가짐, 수많은 스트림을 제공 할 수 있지만 역압력에 의해 제공 속도 제한
- Subscriber : Publisher 가 발행한 이벤티 리스너로 자신을 등록 
- Subscription : 위의 둘 사이의 제어 흐름 즉 역압력을 관리

```java
@FunctionalInterface
public Interface Publiser<T> {
	void subscribe(Subscriber<? super T> s);
}
@FunctionalInterface
public Interface Subsciber<T> {
	void onSubscribe(Subscription s);
	void onNext(T t);
	void onError(Throwable t);
	void onComplete();
} 
```

-  호출 순서 : OnSubscribe  > onNext* > (onError | onComplete) ?
-  Publisher가 자신을 등록 할 때 onSubscribe 메서드를 호출하고 Subscription 객체를 전달한다.

```java
public interface Subscription {
	void request(long n);
	void cancel();
}
```

-  request : Publisher 에게 주어진 개수의 이벤트를 처리 할 수 있음을 알린다.
-  cancel : 이벤트를 받지 않음을 Publisher 에게 통보한다
- Publisher 와 Subscriber 는 정확하게 Subscription을 공유해야한다.

```java
public interface Processor<T,R> extends Subscriber<T>, Publisher<R> {}
```

- Processor 는 에러를 수신하면 onError 신호를 모든 Subscriber에 에러를 전파한다.


### 3. 정리

- 간단히 말해서 리액티브 프로그래밍은 **생성자가 소비자를 압도하지 못하게 하는 목적**
- 동시성 != 많은 쓰레드
- CPU 충분 But 쓰레드 모자람 -> 처리율 저하 -> 쓰레드를 늘리면 -> CPU 부하로 인한 성능 저하


**스프링 리액티브를 쓰지 말아야하는 이유**
- 스프링 개발자가 아니라면
- 스프링 MVC로 개발해서 별 문제 없이 잘 돌아간다면
- 블로킹 IO 작업이 있다면
- 블로킹이 뭔지는 모르겠으나 **JPA, JDBC, MyBatis**를 쓰고 있다면
- 리모트 서비스, API 호출이 전혀 없고, NoSQL도 사용하지 않고, 메세징 서비스 등을 사용하지 않는다면
- 개발팀이 크고 새롭고 도전적인 기술 학습과 시행 착오에 대한 **부담이 있다면**

**스프링 리액티브 개발을 시작할때 기억할 것**

- 비동기-논블로킹 서비스를 만드는 것이 목적
- Publisher, Subscriber를 직접 만드는 경우는 거의 없다
- `` `scribe()` ``는 스프링 MVC, WebFlux가 담당
- 비동기 – 논블로킹 Publihs 생성은 Reactor 라이브러리가 담당



