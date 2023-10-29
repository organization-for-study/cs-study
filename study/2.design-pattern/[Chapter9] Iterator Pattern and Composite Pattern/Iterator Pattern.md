**반복자 패턴(Iterator Pattern)**
----
> 컬렉션의 구현 방법(리스트, 스택, 트리 등)​을 노출하지 않고 그들을 하나씩 순회할 수 있도록 하는 디자인 패턴이다.
<br>

**Collection?**<br>
많은 수의 데이터를 그 사용 목적에 적합한 자료구조로 묶어 하나로 그룹화한 객체를 말한다. <br>
* Collection 종류 : List, Stack, Tree 등..
<img src = "https://refactoring.guru/images/patterns/diagrams/iterator/problem1-2x.png" >
<br>
* 순서가 연속적으로 저장되는 컬렉션들은 for문으로 쉽게 순회가 가능하다.
* 순서가 정해지지 않고 저장되는 컬렉션들은 접근 방식이 다양하기 때문에 순회 방식이 복잡하다.
<img src = "https://refactoring.guru/images/patterns/diagrams/iterator/problem2-2x.png">
<br>

>❗️ 자바의 컬렉션 프레임워크(JCF)에서 각종 컬렉션을 무리없이 순회할수 있는 것도 내부에 미리 이터레이터 패턴이 적용되어 있기 때문이다.

<br>

**반복자 패턴 구조**
<img src = "https://t1.daumcdn.net/cfile/tistory/22266F4557751E6227">
1. Aggregate (인터페이스)
   * ConcreateIterator 객체를 반환하는 인터페이스를 제공한다.
   * iterator() : ConcreateIterator 객체를 만드는 팩토리 메서드
2. ConcreateAggregate (클래스)
   * 여러 요소들이 이루어져 있는 데이터 집합체이다.
3. Iterator (인터페이스)
   * 집합체 내의 요소들을 순서대로 검색하기 위한 인터페이스를 제공한다.
   * hasNext() : 순회할 다음 요소가 있는지 확인한다. (true/false)
   * next() : 요소를 반환하고 다음 요소를 반환할 준비를 하기 위해 커서를 이동시킨다.
4. ConcreateIterator (클래스)
   * 반복자 객체
   * ConcreateAggregate가 구현한 메서드로부터 생성되며, ConcreateAggreate의 컬렉션을 참조하여 순회한다.
   * 어떤 전략으로 순회할지에 대한 로직을 구체화 한다.
  
<br>

**반복자 패턴 적용** <br>
1. 컬렉션의 복잡한 내부 구조를 보안이나 편의상으로 클라이언트로부터 숨기고 싶은 경우
2. 순회 코드의 중복을 줄이고 싶은 경우
3. 컬렉션에 상관없이 객체 접근 순회 방식을 통일하고자 하는 경우
4. 데이터 저장 컬렉션 종류가 변경될 가능성이 있는 경우
   
<br>

**반복자 패턴의 장점** <br>
1. 일관된 이터레이터 인터페이스를 사용해 여러 형태의 컬렉션에 대해 동일한 순회 방법을 제공한다.
   * 컬렉션의 내부 구조 및 순회 구조를 모르고 있어도 된다.
2. 집합체의 구현과 접근하는 처리 부분을 반복자 객체로 분리해 결합도를 줄 일 수 있다.
   * Client에서 iterator로 접근하기 때문에 ConcreateAggregate 내에 수정 사항이 생겨도 iterator에 문제가 없다면 문제가 발생하지 않는다.
3. 단일 책임 원칙 준수 (SRP)
   * 순회 알고리즘을 별도의 클래스로 추출하여 클라이언트 코드와 컬렉션들을 정돈할 수 있다.
4. 개방/폐쇄 원칙 준수 (OCP)
   * 새로운 컬렉션과 반복자를 구현할 수 있으며 기존 구현 코드는 손상되지 않는다.
  
<br>

**반복자 패턴의 단점** <br>
1. 클래스가 늘어나고 복잡도가 증가한다.
2. 구현 방법에 따라 캡슐화에 위배될 수 있다.
  
<br>

**복합 패턴(반복자 패턴 + OO패턴)** <br>
1. 팩토리 메서드를 반복자와 함께 사용하여 컬렉션 자식 클래스들이 해당 컬렉션들과 호환되는 다양한 유형의 반복자들을 반환하도록 할 수 있습니다.
2. 메멘토 패턴을 반복자 패턴과 함께 사용하여 현재 순회 상태를 포착하고 필요한 경우 롤백할 수 있습니다.
3. 비지터 패턴과 반복자 패턴을 함께 사용해 복잡한 데이터 구조를 순회하여 해당 구조의 요소들의 클래스들이 모두 다르더라도 이러한 요소들에 대해 어떤 작업을 실행할 수 있습니다.
<br>

**패턴이 사용되는 예**<br>
1. Java
   1. java.util.Enumeration
    * Iterator가 만들어지기 전 java 1.0부터 있었던 API
    * 과거의 클래스로서, 현재는 안쓰고 Iterator로 기능이 대체 되었다.
    * java 9 에서는 Enumeration Iterator로 변환해주는 코드가 추가되었다.
   2. java.util.Iterator
    * hasNext() : 순회할 요소가 있는지 확인
    * next() : 현재 커서의 요소를 출력하고 다음으로 커서를 이동
    * remove() : Iterator에서 next()로 받았던 해당 엘리먼트를 삭제
    * forEachRemaining() : 함수형 인터페이스를 통해 순회 코드를 심플화 해준다
   3. Java StAX (Streaming API for XML)
    * XML 파일 포맷을 읽거나 만들때 사용하는 자바 라이브러리
    * 콘솔 기반의 API, 이터레이터 기반의 API를 제공한다.
    * 비슷한 SAX(Simple API for XML)도 있지만, SAX는 XML을 읽기만 가능하다.
2. Spring Framework
   1. CompositeIterator
    * 기존의 Interator에 add 기능만 하나 추가한 것
    * add() : 여러 Iterator들을 조합(Composite)해서 사용할 수 있다.
