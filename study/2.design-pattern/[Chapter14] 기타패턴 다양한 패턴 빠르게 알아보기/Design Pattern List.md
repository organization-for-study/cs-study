**기타 패턴 : 다양한 패턴 빠르게 알아보기**
----
<br>

**브리지 패턴**
> 큰 클래스 또는 밀접하게 관련된 클래스들의 집합을 두 개의 개별 계층구조(추상화 및 구현)로 나눈 후 각각 독립적으로 개발할 수 있도록 하는 구조 디자인 패턴

<img src = "https://blog.kakaocdn.net/dn/dD0SuR/btqwsXr3dGN/gcLPkY2KvJOiW1nqUGkQsk/img.png">

1. 활용법
   * 여러 플랫폼에서 사용해야 하는 그래픽스와 윈도우 처리 시스템에서 활용된다.
   * 인터페이스와 실제 구현할 부분을 서로 다른 방식으로 변경해야 할 때 유용하게 쓰인다.
2. 장점
   * 구현과 인터페이스를 완전히 결합하지 않았기에 구현과 추상화 부분을 분리할 수 있다.
   * 추상화된 부분과 실제 구현 부분을 독립적으로 확장할 수 있다.
   * 추상화 부분을 구현한 구상 클래스가 바뀌어도 클라이언트에는 영향을 끼치지 않는다.
3. 단점
   * 디자인이 복잡해진다.


<br>

**빌더 패턴**
> 복잡한 객체들을 단계별로 생성할 수 있도록 하여 같은 제작 코드를 사용하여 객체의 다양한 유형들과 표현을 제작할 수 있는 디자인 패턴

<img src = "https://images.velog.io/images/hero6027/post/dbbe5291-54a9-4324-b2a3-34df05096f42/builderGof.png">

1. 활용법
   * 복합 객체 구조를 구축하는 용도로 많이 쓰인다.
2. 장점
   * 복합 객체 생성 과정을 캡슐화한다.
   * 여러 단계와 다양한 절차를 거쳐 객체를 만들 수 있다.
   * 제품의 내부 구조를 클라이언트로부터 보호할 수 있다.
   * 클라이언트는 추상 인터페이스만 볼 수 있기에 제품을 구현한 코드를 쉽게 바꿀 수 있다.
3. 단점
   * 팩토리를 사용할 때 보다 객체를 만들 때 클라이언트에 관해 더 많이 알아야 한다.


<br>

**책임 연쇄 패턴**
> 핸들러들의 체인을 따라 요청을 전달할 수 있게 해주는 패턴으로 각 핸들러는 요청을 받으면 요청을 처리할지 아니면 체인의 다음 핸들러로 전달할지를 결정

<img src = "https://t1.daumcdn.net/cfile/tistory/2503383F572056D303">

1. 활용법
   * 윈도우 시스템에서 마우스 클릭과 키보드 이벤트를 처리할 때 사용된다.
2. 장점
   * 요청을 보낸 쪽과 받는 쪽을 분리할 수 있다.
   * 객체는 사슬의 구조를 몰라도 되고 그 사슬에 들어있는 다른 객체의 직접적인 레퍼런스를 가질 필요도 없으므로 객체를 단순하게 만들 수 있다.
   * 사슬에 들어가는 객체를 바꾸거나 순서를 바꿈으로써 역할을 동적으로 추가하거나 제거할 수 있다.
3. 단점
   * 요청이 반드시 수행된다는 보장이 없다. 사슬 끝까지 갔는데도 처리되지 않을 수 있다. (장점일 수도 있다.)
   * 실행 시 과정을 살펴보거나 디버깅하기가 힘들다.


<br>

**인터프리터 패턴 패턴**
> 간단한 언어의 문법을 정의하고 해석하는 디자인 패턴으로 자주 등장하는 문제를 간단한 언어로 정의하고 재사용할 수 있음

<img src = "https://velog.velcdn.com/images/weekbelt/post/57c69679-02fd-45d7-b354-2e60e32085f9/image.png">

1. 활용법
   * 간단한 언어를 구현할 때 인터프리터 패턴이 유용하게 쓰인다.
   * 효율보다는 단순하고 간단하게 문법을 만드는 것이 더 중요한 경우에 유용하다.
   * 스크립트 언어와 프로그래밍 언어에서 모두 쓸 수 있다.
2. 장점
   * 문법을 클래스로 표현해서 쉽게 언어를 구현할 수 있다.
   * 문법이 클래스로 표현되므로 언어를 쉽게 변경하거나 확장할 수 있다.
   * 클래스 구조에 메소드만 추가하면 프로그램을 해석하는 기본 기능 외에 예쁘게 출력하는 기능이나 더 나은 프로그램 확인 기능 같은 새로운 기능을 추가할 수 있다.
3. 단점
   * 문법 규칙의 개수가 많아지면 아주 복잡해진다. (이런 경우 파서나 컴파일러 생성기를 쓰는 편이 좋다.)


<br>

**중재자 패턴**
> 객체 간의 혼란스러운 의존 관계들을 줄일 수 있는 패턴으로 객체 간의 직접 통신을 제한하고 중재자 객체를 통해서만 협력하도록 함

<img src = "https://t1.daumcdn.net/cfile/tistory/999B17425C620C4E24">

1. 활용법
   * 서로 연관된 GUI 구성 요소를 관리하는 용도로 많이 쓰인다.
2. 장점
   * 시스템과 객체를 분리함으로써 재사용성을 획기적으로 향상시킬 수 있다.
   * 제어 로직을 한 군데 모아놨으므로 관리하기가 수월하다.
   * 시스템에 들어있는 객체 사이에서 오가는 메시지를 확 줄이고 단순화할 수 있다.
3. 단점
   * 디자인을 잘 하지 못하면 중재자 객체가 너무 복잡해진다.


<br>

**메멘토 패턴**
> 객체의 구현 세부 사항을 공개하지 않으면서 해당 객체의 이전 상태를 저장하고 복원할 수 있게 해주는 디자인 패턴

<img src = "https://velog.velcdn.com/images/weekbelt/post/c14c0d5b-3d44-4848-9ee3-b8521e82fb8b/image.png">

1. 활용법
   * 메멘토 객체를 써서 상태를 저장한다.
   * 자바 시스템에서는 시스템의 상태를 저장할 때 직렬화를 사용하는 것이 좋다.
2. 장점
   * 저장된 상태를 핵심 객체와는 다른 별도의 객체에 보관할 수 있어 안전하다.
   * 핵심 객체의 데이터를 계속해서 캡슐화된 상태로 유지할 수 있다.
   * 복구 기능을 구현하기가 쉽다.
3. 단점
   * 상태를 저장하고 복구하는 데 시간이 오래 걸릴 수 있다.


<br>

**프로토타입 패턴**
> 코드를 그들의 클래스들에 의존시키지 않고 기존 객체들을 복사할 수 있도록 하는 디자인 패턴

<img src = "https://www.baeldung.com/wp-content/uploads/2019/10/Prototype-Pattern.png">

1. 활용법
   * 시스템에서 복잡한 클래스 계층구조에 파뭍혀 있는 다양한 형식의 객체 인스턴스를 새로 만들어야 할 때 유용하게 쓸 수 있다.
2. 장점
   * 클라이언트는 새로운 인스턴스를 만드는 과정을 몰라도 된다.
   * 클라이언트는 구체적인 형식을 몰라도 객체를 생성할 수 있다.
   * 상황에 따라서 객체를 새로 생성하는 것보다 객체를 복사하는 것이 더 효율적일 수 있다.
3. 단점
   * 복사본을 만드는 일이 매우 복잡할 수 있다.


<br>

**비지터 패턴**
> 알고리즘들을 그들이 작동하는 객체들로부터 분리할 수 있도록 하는 디자인 패턴

<img src = "https://fifabell.github.io/assets/images/VisitorUML.png">

1. 장점
   * 구조를 변경하지 않으면서도 복합 객체 구조에 새로운 기능을 추가할 수 있다.
   * 비교적 손쉽게 새로운 기능을 추가할 수 있다.
   * 비지터가 수행하는 기능과 관련된 코드를 한곳에 모아 둘 수 있다.
2. 단점
   * 비지터를 사용하면 복합 클래스의 캡슐화가 깨진다.
   * 컬랙션 내의 모든 항목에 접근하는 트래버서가 있으므로 복합 구조를 변경하기가 더 어려워진다.
  
<br>
<hr>
<br>

**패턴 분류**
1. 생성 패턴
   * 기존 코드의 유연성과 재사용을 증가시키는 다양한 객체 생성 메커니즘들을 제공한다.
   * 패턴 종류 : 팩토리 메서드, 추상 팩토리, 빌더, 프로토타입, 싱글턴
2. 구조 패턴
   * 객체들과 클래스들을 구조를 유연하고 효율적으로 유지하면서 더 큰 구조로 조립하는 방법을 설명한다.
   * 패턴 종류 : 어댑터, 브리지, 복합체, 데코레이터, 퍼사드, 플라이웨이트, 프록시
3. 행동 패턴
   * 알고리즘들 및 객체 간의 책임 할당과 관련이 있다.
   * 패턴 종류 : 책임 연쇄, 커맨드, 반복자, 중재자, 메멘토, 옵서버, 상태, 전략, 템플릿 메서드, 비지터, 인터프리터