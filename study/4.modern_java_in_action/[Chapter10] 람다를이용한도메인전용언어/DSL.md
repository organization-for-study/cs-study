## 람다를 이용한 도메인 전용 언어

### DSL
#### DSL이란?
> 특정 비즈니스 도메인의 문제를 해결하려고 만든 언어<br>
- 특정 비즈니스 도메인을 인터페이스로 만든 API
- 특정 도메인에 국한

#### DSL 개발 필요성
- 코드의 의도가 명확히 전달되어야 하고 프로그래머가 아닌 사람도 이해할 수 있어야 함
- 가독성은 유지보수의 핵심

#### DSL의 장점과 단점
> DSL을 도메인에 이용하면 약이 되거나 독이 될 수 있음 <br>
- 장점
  - 간결함
    - 도메인 영역의 용어 사용으로, 비 도메인 전문가도 이해가 쉬움
    - 다양한 조직 구성원 간에 코드와 도메인 영역이 공유될 수 있음
  - 가독성
    - 도메인 영역의 용어를 사용하므로 비 도메인 전문가도 코드를 쉽게 이해할 수 있음
    - 결과적으로 다양한 조직 구성원 간에 코드와 도메인 영역이 공유될 수 있음
  - 유지보수
    - 잘 설계된 DSL로 구현한 코드는 쉽게 유지보수 가능
    - 지정된 언어로 비즈니스 로직을 표현함으로 애플리케이션의 인프라구조와 관련된 문제와 독립적으로 비즈니스 관련된 코드에 집중하기 용이
  - 높은 수준의 추상화
    - DSL은 도메인과 같은 추상화 수준에서 동작하므로 도메인의 문제와 직접적으로 관련되지 않은 세부사항을 숨김
  - 집중
    - 비즈니스 도메인의 규칙을 표현할 목적으로 설계된 언어이므로 프로그래머가 특정코드에 집중할 수 있어 생산성 증가
- 단점
  - DSL 설계의 어려움
    - 간결하게 제한적인 언어에 도메인 지식을 담는 것이 쉬운 작업이 아님
  - 개발 비용
    - 코드에 DSL을 추가하는 작업은 초기 프로젝트에 많은 비용과 시간이 소모되는 작업
    - DSL 유지보수와 변경은 프로젝트에 부담을 주는 요소
  - 추가 우회 계층
    - DSL은 추가적인 계층으로 도메인 모델을 감싸며 이 때 계층을 최대한 작게 만들어 성능 문제를 회피
  - 새로 배워야 하는 언어
  - 호스팅 언어의 한계
    - 사용자 친화적 DSL을 만들기가 힘듦
    - 람다 표현식은 이 문제를 해결하는 강력한 도구

#### DSL 패턴 종류
- 메서드 체인(플루언트 스타일)을 이요한 DSL
- 중첩된 함수 이용
- 람다 표현식을 이용한 시퀀싱

#### 메서드 체인(플루언트 스타일)을 이요한 DSL
> DSL을 구현하는 가장 흔한 방식으로, 인스턴스의 생선단계를 Builder 클래스에 구현해서 지정된 절차에 따라 API를 호출하도록 할 수 있음 <br>

#### 중첩된 함수 이용
> 다른 함수안에 함수를 이용해서 도메인을 만듦 <br>

#### 람다 표현식을 이용한 시퀀싱
> 람다 표현식으로 정의한 함수 시퀀스를 사용 <br>

#### DSL 패턴 장단점
<img src = "https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1846146b-0402-4ca7-8709-5e0a5d09c612%2FUntitled.png&blockId=a5a4831d-db25-434f-8476-c7ea8434677a">

### 쿼리 DSL

#### 쿼리 DSL이 필요한 이유
> 객체 지향 언어는 객체를 생성하고 객체간 관계를 맺는 반면에 관계형 DB는 테이블간 외래키로 관계를 맺음 <br>
> 관계형 DB와 어플리케이션은 패러다임이 다르기에 중간에 변환 과정이 필요 <br>
##### JDBC(Java Database Connectivity)의 등장
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fz9Nw8%2Fbtsgte9IXWm%2FyN4cwDjUp4TqmTZSy3uvRk%2Fimg.png" width="70%"/> <br>

- 어플리케이션에서 DB에 읽기/쓰기를 하기 위해 두 환경을 연결하는 모듈을 자바 언어로 구현한 라이브러리
- iBatis, MyBatis 같은 ORM 기술이 등장하며 JDBC를 조금 더 추상적으로 사용할 수 있게 됌
- 하지만 패러다임 불일치 문제는 해결 되지 않음

##### 패러다임 불일치
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDau6l%2FbtsguC9Girx%2FCQvcL6kQ8qI8b9TLPoYrvK%2Fimg.png" height = "300px" style="float:left;"><img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FT3XPs%2FbtsgmEPmnjw%2Fykbp60ePSAre1O6Aq95PyK%2Fimg.png" height = "250px" style="float:left;margin-left :10px;"><div style="clear:both"></div><br>

- '상속'의 개념 유무
  - 객체지향언어에서는 상속의 개념이 존재하기 때문에 객체를 물리적으로 만들지 않아도 사용할 수 있음
  - DB환경에서는 물리적으로 만들어야함
  
##### JPA가 필요한 이유
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFSr5N%2FbtsgttMC4J1%2F4tqn6YRNJO8CNmiJltOp91%2Fimg.png" width="50%"> <br>

- 패러다임 불일치 문제 해결
- 어플리케이션과 DB 사이의 불일치를 개발자가 직점 풀어주지 않고 자동화함
- SQL 중심 프로그래밍에서 객체지향중심 프로그래밍으로 전환됨 

##### JPQL이란?
- Java Persistence Query Language
- 모든 쿼리를 객체지향방식으로 표현할 수 없는 JPA의 한계점을 보완
- 조건이 여러개 붙는 경우나 동적으로 변하는 경우 사용
- SQL 포맷은 유지하되, 테이블이 아닌 <b>엔티티</b>를 대상으로 함
- SELECT문 = SELECT절 FROM절 [ WHERE 절 ] [GROUPBY 절 ] [ HAVING 절 ] [ ORDERBY 절 ]

##### 쿼리 DSL이 필요한 이유
> JPQL은 SQL문과 상당히 유사하나 문제점이 있음 <br>
1. 타입 안정성이 떨어짐
   - JPQL은 문자열이기에 개발자는 문법이 틀려도 이를 알아차리지 못함 -> 런타임 중에 메소드가 호출되어 파싱되어야 문법 오류를 발견할 수 있음
   - 즉, JPQL을 파싱하는 프로세스가 동작해야만 문법오류를 발견할 수 있고 컴파일 과정에서는 오류 발견이 어려움
2. 직관적인 동적쿼리 작성이 어려움
   - 동적쿼리를 작성하려면 문자열을 조작하는 방식으로 로직을 구성해야함
   - 문자열과 문자열 사이에는 if-else문이나 for문 같은 코드가 들어가 가독성을 떨어트림

#### 쿼리 DSL이란?
- SQL, JPQL을 코드로 작성할 수 있도록 도와주는 오픈소스 빌더 API
- 컴파일 시점에 문법 오류를 발견할 수 있음
- IDE의 도움을 받아 코드 자동완성 기능을 이용할 수 있음
- 엔티티를 기반으로 생성한 쿼리 타입이라는 쿼리용 클래스(QClass)를 사용
> QClass란? <br>
> QueryDSL은 컴파일 단계에서 엔티티를 기반으로 QClass를 생성하는데 JPAAnnotationProcessor 가 컴파일 시점에 작동해서 @Entity 등등의 어노테이션을 찾아 해당 파일들을 분석해서 QClass 를 만듦<br>
> QClass 는 Entity 와 형태가 똑같은 Static Class임<br>
> QueryDSL 은 쿼리를 작성할 때 QClass 를 기반으로 쿼리를 실행함 <br>


#### 쿼리 DSL 동작 원리
##### 기존 JPQL이 수행되는 원리
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn1Fhy%2FbtspfOB1wZE%2FSIp6HuatBOW3M4bFpAdf91%2Fimg.png" width="70%"> <br>

##### QueryDSL에서 JPQL이 실행되는 원리
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnI4T8%2FbtspeIPCLp0%2FpmndIdQ8Q3VbuNHFFUgUt1%2Fimg.png" width="70%"> <br>
- QueryDSL의 목적 : JSQL 생성


#### 쿼리 DSL 작성 예시
~~~
select m from Member m where m.age > 14


// QueryDSL 사용
EntityManager em = emf.createEntityManager();
JPAQuery query = new JPAQuery(em); // com.mysema.query.jpa.imlp.JPAQuery
QMember qm = new QMember("m"); // 자동으로 생성되는 JPQL에서 사용될 별칭을 "m"으로 지정하면서 Member에 대한 쿼리 타입을 생성
// QMember qm = QMember.member; // 별칭을 지정하지 않고 기본 인스턴스를 사용하는 방법

List<Member> list = query.from(qm)
    	.where(qm.age.gt(14))
        .orderBy(qm.name.desc())
        .list(qm);
~~~

1. 검색 조건 쿼리
   - where()에 작성
   - 내부에 and()나 or() 사용 가능
    ~~~
    .where(qm.name.like("kim%").and(qm.age.gt(20))
    
    .where(qm.name.like("kim%"), qm.age.gt(20))
    ~~~
2. 결과 조회
   - 파라미터로 프로젝션 대상을 넘겨주면 결과 조회 가능
   - uniqueResult() 
     - 조회 결과가 한 건일 때 사용
     - 조회 결과 없을 때 null 반환
     - 조회 결과 하나 이상일 때 com.mysema.query.NonUniqueResultException 예외 발생
   - list()
     - 결과가 하나 이상이 ㄹ때 사용
     - 조회 결과 없을 때 빈 컬렉션 반환
   - singleResult()
     - 맨 처음 데이터만 반환
3. 페이징과 정렬
   - orderBy()에 작성
   - 정렬은 asc(), desc() 사용
   - 페이징 
     - offset(), limit() 조합해서 사용
     - restrict() 메소드에 QueryModifiers를 파라미터로 넣어서 사용
    ~~~
    SearchResults<Item> result = query.from(qitem)
        .where(qitem.price.gt(20000))
        .offset(20).limit(10)
        .listResults(qitem); // 전체 데이터 수를 알고 싶은 경우

    long total = result.getTotal(); // 검색된 데이터 수
    List<Item> results = result.getResults();
    ~~~
4. 그룹
   - groupBy()에 작성
   - having()을 사용하여 그룹화된 결과에 조건 가능
5. 조인
   - join()(innerJoin()), innerJoin(), leftJoin(), rightJoin(), fullJoin()에 작성
   - on()을 사용하여 조인 조건 생성
   - fetch()를 사용하여 fetch 조인 가능
    ~~~
    Order qo = QOrder.order;
    QMember qm = QMember.member;
    QOrderItem qoi = QOrderItem.orderItem;

    List<Order> list = query.from(qo)
        .join(qo.member, qm)
    .on(qoi.count.gt(2))
    .list(qo);
    ~~~
6. 서브 쿼리
   - com.mysema.query.jpa.JPASubQuery를 사용
   - 서브 쿼리의 결과가 하나면 unique(), 여러 개면 list()를 사용
    ~~~
    .where(qitem.price.eq(
    new JPASubQuery().from(qitemSub)
        ...
        .unique(qitemSub.price.max())
    ))
    ~~~
7. 프로젝션
   - 프로젝션 대상이 하나인 경우
     - 해당 타입으로 반환
        ~~~
        List<String> result = query.from(qitem).list(qitem.name);
        ~~~
   - 프로젝션 대상이 둘 이상인 경우
     - com.mysema.query.Tuple 타입을 사용
        ~~~
         List<Tuple> result = query.from(qitem).list(qitem.name, qitem.price); 
        for(Tuple tuple : result) {
            String name = tuple.get(qitem.name);
            int price = tuple.get(qitem.price);
        }
        ~~~
   - DTO로 받기
     - 쿼리 결과를 엔티티가 아닌 특정 객체로 받을 수 잇음
     - 엔티티의 필드명과 DTO의 필드명이 다르다면 as() 메소드를 이용해서 DTO의 필드명으로 맞춰주면 됌
     - 프로퍼티(setter) 접근 방식
       - Projections.bean() 메소드를 사용
       ~~~
        List<ItemDTO> result = query.from(qitem)
          .list(
              Projection.bean(ItemDTO.class, qitem.name.as("username"), qitem.price)
          );
       ~~~
     - 필드 직접 접근 방식
       - Projections.fields() 메소드를 사용
       - 필드의 접근자를 private로 지정해도 동작
       ~~~
        List<ItemDTO> result = query.from(qitem)
          .list(
              Projection.fields(ItemDTO.class, qitem.name.as("username"), qitem.price)
          );
       ~~~
     - 생성자 사용방식
       - Projections.constructor() 메소드 사용
       - 엔티티의 필드명과 DTO의 필드명이 다르더라도 as() 메소드르 사용하지 않아도 됌
       - 파라미터의 순서만 같으면 됌
       ~~~
        List<ItemDTO> result = query.from(qitem)
            .list(
                Projection.constructor(ItemDTO.class, qitem.name, qitem.price)
            );
       ~~~
   - DISTINCT
     ~~~
      query.distinct().from(qitem)...
     ~~~
8. 배치 쿼리
   - QueryDSL은 여러 연산을 일괄적으로 처리하는 배치 쿼리를 지원함
   - 영속성 컨텍스트를 무시하고 <b>DB에 직접 쿼리함</b>
   - 영향 받은 튜플의 수가 반환됌
   - 수정 배치 쿼리
     - com.mysema.query.jpa.impl.JPAUpdateClause를 사용
       ~~~
        JPAUpdateClause updateClause = new JPAUpdateClause(em, qitem);
        long count = updateClause.where(qitem.name.eq("자바 ORM 표준 JPA 프로그래밍"))
            .set(qitem.price, qitem.price.add(1000)) // Item의 price를 1000 증가
            .execute();
       ~~~
   - 삭제 배치 쿼리
     - com.mysema.query.jpa.impl.JPADeleteClause를 사용
       ~~~
        JPADeleteClause deleteClause = new JPADeleteClause(em, qitem);
        long count = deleteClause.where(qitem.name.eq("자바 ORM 표준 JPA 프로그래밍"))
            .execute();
       ~~~
       
10.  동적쿼리
    - com.mysema.query.BooleanBuilder는 where()에 들어가는 조건을 동적으로 담는 빌더 객체
     ~~~
      BooleanBuilder builder = new BooleanBuilder();
      builder.and(qitem.name.contains("우유"));
      builder.and(qitem.price.gt(2500));
      
      List<Item> result = query.from(qitem)
          .where(builder)
          .list(qitem);
     ~~~
12. 메소드 위임
    - 메소드 위임 기능이란?
      - 자주 사용하는 검색 조건이 있다면 쿼리 타입에 검색 조건을 직접 추가 가능
    - 검색 조건 정의
      - @com.mysema.query.annotations.QueryDelegate 어노테이션을 이용
      - @QueryDelegate의 속성에 엔티티 클래스를 지정
      - 정적 메소드의 첫 번째 파라미터에는 대상 엔티티의 쿼리 타입 객체를 지정하고, 두 번째 파라미터부터는 필요한 파라미터를 지정 할 수 있음
      - String, Date와 같은 자바 기본 ㅐㄴ장 타입에도 메소드 위임 기능을 사용할 수 있음
    - 메소드 위임 기능 사용
      - 쿼리 타입의 메소드를 그대로 사용하면 됌
        ~~~
        List<Item> result = query.from(qitem)
            .where(qitem.isExpensive(2400))
            .list(qitem);
        ~~~

> 출처 <br>
> Query DSL : https://kimcoder.tistory.com/495<br>
> Query DSL : https://lordofkangs.tistory.com/456<br>
> Query DSL 레퍼런스 문서 : https://querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/ <br>
> JPQL : https://lordofkangs.tistory.com/386<br>
> JPA 패러다임 불일치 : https://lordofkangs.tistory.com/336<br>
