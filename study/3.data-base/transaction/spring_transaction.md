## 트랜잭션 관리와 무결성

### 트랜잭션 관리를 위한 기술과 알고리즘
#### 트랜잭션 전파
<a href="https://hstory0208.tistory.com/entry/Spring-Transactional-%EC%98%B5%EC%85%98-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%A0%84%ED%8C%8C">트랜잭션전파</a> <br>

#### JPA 트랜잭션 관리
- @Transaction 이란?
  > 데이터베이스 작업에서 트랜잭션을 관리하는 데 사용되는 어노테이션
  - 선언적 트랜잭션
  - 클래스 또는 메서드 레벨에 적용할 수 있다.
  - 해당 scope에 프록시를 만들어 비즈니스 로직을 수행한다.
  - 예시) <a href="https://kghworks.tistory.com/106">@Transaction 예시</a>
- @Transaction 설정값
  - @Transaction(readOnly = true)
    - readOnly로 설정했다면 해당 Transaction 안에서는 insert, delete, update query가 불가능하다. 즉, 읽기 전용으로 설정 가능하다.
  - @Transaction(isolation = Isolation.DEFAULT)
    - Transaction의 격리 수준을 설정할 수 있다.
  - @Transcation(rollbackFor=Exception.class), @Transcation(noRollbackFor=Exception.class)
    - runtiem Exception 발생시 rollback한다. 혹은, 특정 예외 발생 시 rollback 하지 않도록 할 수 있다.
  - @Transaction(timeout=10)
    - 설정한 시간안에 트랜잭션을 롤백시키며 트랜잭션 예외(트랜잭션 시간 만료 오류)가 발생한다.
    - 시간값은 정수여야 하고 밀리초 단위로 간주된다.
    - timeout의 default 값은 -1이기 때문에 default로 timeout이 지원되지 않느다.

> 출처<br>
> <a href="https://kghworks.tistory.com/106">@Transaction이란1</a><br>
> <a href="https://colevelup.tistory.com/34">@Transaction이란2</a><br>
> <a href="https://velog.io/@kim_think_rae/JPA-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B4%80%EB%A6%AC">@Transaction 설정값</a><br>
<br>
> Spring Transaction 참고 내용
> <a href="https://mangkyu.tistory.com/154">트랜잭션에 대한 이해와 Spring이 제공하는 Transaction(트랜잭션) 핵심 기술</a><br>
> <a href="https://mangkyu.tistory.com/169">Spring 트랜잭션의 세부 설정(전파 속성, 격리수준, 읽기전용, 롤백/커밋 예외 등)</a><br>
> <a href="https://mangkyu.tistory.com/170">Spring에서 트랜잭션의 사용법</a><br>
