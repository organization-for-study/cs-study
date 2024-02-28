## 람다를 이요한 도메인 전용 언어

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
     - 필드 직접 접근 방식
     - 생성자 사용방식
   - DISTINCT
8. 배치 쿼리
9.  동적쿼리
10. 메소드 위임

> 출처 <br>
> Query DSL : https://kimcoder.tistory.com/495<br>
> Query DSL : https://lordofkangs.tistory.com/456<br>
> JPQL : https://lordofkangs.tistory.com/386<br>
> JPA 패러다임 불일치 : https://lordofkangs.tistory.com/336<br>