## 데이터베이스 관리 시스템(DBMS)의 기본 개념

### 데이터베이스의 개념
#### 데이터베이스 개념
1. 정의
   - 데이터들을 저장하고 조회하는 프로그램
2. 데이터베이스 데이터 특징
   - 통합된 데이터
      - 중복된 정보에 대해서 데이터를 통합하여 자료의 중복을 최소화한 데이터의 모임
   - 저장된 데이터
      - 컴퓨터가 접근할 수 있는 매체에 데이터 저장
   - 운영 데이터
      - 조직의 목적을 위해 존재하고 활용되는 운영 데이터를 다루는데 주로 이용
   - 공유 데이터
      - 여러 사람들이 공유하고 사용할 목적으로 통합 관리되는 데이터
2. 기능
   - 실시간 접근성
      - 사용자의 요구에 신속하고 정확하게 응답이 가능해야 함
   - 계속적인 변화
      - 현실 세계의 변화를 반영하기 위해 새로운 데이터의 삽입, 삭제, 갱신으로 항상 최신 데이터를 유지하는 것
   - 동시 공용
      - 다수의 사용자가 동시에 같은 내용의 데이터를 이용할 수 있어야 함 
   - 내용에 의한 참조
      - 데이터베이스에 있는 데이터를 참조할 때 사용자의 요구에 따른 데이터 내용을 데이터의 위치나 주소로 찾음
3. 데이터베이스의 언어 SQL
   - 관계형 데이터베이스에서 데이터를 정의하고 조작하고 제어할 수 있는 표준 언어
   - DDL(Data Definition Language)
      - 테이블과 같은 데이터 구조를 정의하는데 사용하는 명령어들
      - CREATE, ALTER, DROP, RENAME, TRUNCATE
   - DML(Data Manipulation Language)
      - 데이터를 조회, 변형하는 명령어들
      - SELECT, INSERT, UPDATE, DELETE
   - DCL(Data Control Language)
      - 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어들
      - GRANT, REVOKE
   - TCL(TRansaction Control Language)
      - 논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 작업단위(트랜잭션) 별로 제어하는 명령어들
      - COMMIT, ROLLBACK, SAVEPOINT 

#### 데이터베이스 기본 구조
1. 스키마 (Schema)
   - 데이터베이스에서 자료의 구조, 자료의 표현 방법, 자료 간의 관계를 형식 언어로 정의한 구조
   - DBMS(DataBase Management System, 데이터베이스 관리 시스템)이 주어진 설정에 따라 데이터베이스 스키마를 생성하며, 데이터베이스 사용자가 자료를 저장, 조회, 삭제, 변경할 때 DBMS는 자신이 생성한 데이터베이스 스키마를 참조하여 명령을 수행함
   - 스키마의 3층 구조 <br>
    <img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMEgdH%2Fbtqv8xNyg6t%2FiK30LSKBoR32UzaPteYcQk%2Fimg.jpg"> <br>
     - 외부 스키마 (External Schema) : 프로그래머나 사용자의 입장에서 데이터베이스의 모습으로 조직의 일부분을 정의한 것
     - 개념 스키마 (Conceptual Schema) : 모든 응용 시스템과 사용자들이 필요로하는 데이터를 통합한 조직 전체의 데이터베이스 구조를 논리적으로 정의한 것
     - 내부 스키마 (Internal Shema) : 전체 데이터베이스의 물리적 저장 형태를 기술하는 것
2. 테이블 (Table)
   - = relation
   - 행과 열로 구성된 정렬된 데이터 집합의 모임
   - 관계형 데이터베이스의 사용자 데이터를 보유하는 기본 구조 
3. 컬럼 (Column)
   - = attribute = 열 = 속성
   - 관계형 데이터베이스 테이블에서 특정한 단순 자료형의 일련의 데이터값과 테이블에서 각 열을 말함
   - 필드(feild)와 필드 값은 한 열이나 한 컬럼 사이의 교차로 존재하는 단일 항목을 특정할 때 언급하는 것으로 Column과 혼동하면 안됌!
   - 컬럼과 필드 차이점 <br>
     
      | 파일 시스템 | 데이터베이스 모델링 | 관계형 데이터베이스 |
      |:---:|:---:|:---:|
      | 파일(file) | 엔티티(Entitiy) | 테이블(table) |
      | 레코드(record) | 튜플(Tuple) | 행(row) |
      | 키(key) | 유일값(Identifier) | 기본키(Primary key), unique | 
      | 필드(field) | 어트리뷰트(attribute) | 컬럼(column) |
4. 로우 (Row)
   - =record = tuple = 행
   - 관계형 데이터베이스에서 레코드 또는 튜플로 불리기도 하며, 어떤 테이블에서 단일 구조 데이터 항목을 가리킨다.
5. 디그리 (Degree)
   - 테이블이 가지고 있는 Column의 수 
6. 카디널리트(Cardinality)
   - 테이블이 가지고 있는 Row의 수

<br>

### 데이터베이스 관리 시스템의 정의와 종류
#### 데이터베이스 관리 시스템 정의와 종류
1. 정의 <br>
   - 데이터베이스를 관리하고 운영하는 소프트웨어
   - 특정 목적을 처리하기 위한 프로그램
  <br> <img src = "https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/DBMS_%EA%B0%9C%EB%85%90.png" width ="50%" ><br>
2. 구성 <br>
   <img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F9QHJh%2Fbtqv7O348Ud%2FChBPHpTRWHo0IYA8OlYuq0%2Fimg.png" width="50%"> <br>
   - 데이터베이스 관리 시스템은 기능에 따라 크게 질의 처리기와 저장 데이터 관리자로 구성되어있다.
   - 질의 처리기
      - 사용자의 데이터 처리 요구를 해석하여 처리하는 역할
      - DDL 컴파일러
         - 데이터 정의어로 작성된 스키마를 해석
         - 데이터베이스를 생성하거나, 스키마의 정의를 데이터 사전에 저장
      - DML 프리컴파일러
         - 응용 프로그램에 삽입된 데이터 조작어를 추출하여 DML 컴파일러에 전달
      - DML 컴파일러
         - 데이터 조작어(삽입, 삭제, 수정, 검색) 요청을 분석하여 런타임 데이터베이스 처리기가 이해할 수 있도록 해석
      - 런타임 데이터베이스 처리기
         - 저장 데이터 관리자를 통해 데이터베이스 접근하여 DML 컴파일러로 부터 전달받은 요청을 데이터베이스에서 실제로 실행
      - 트랜잭션 관리자
         - 데이터베이스에 접근하는 과정에서 사용자의 접근 권한이 유효한지 검사하고, 데이터베이스 무결성을 유지하기 위한 제약조건 위반 여부를 확인
         - 회복이나 병행 수행과 관련된 작업도 함 <br>
      > **컴파일러란**  <br>
      > 특정 프로그래밍 언어로 쓰여 있는 문서를 다른 프로그래밍 언어로 옮기는 프로그램으로 우리가 작성한 고급 프로그래밍 언어는 컴파일러가 컴퓨터가 이해할 수 있는 언어(기계어)로 변환해주어야 실행됨 <br>
   - 저장 데이터 관리자
      - 디스크에 저장되어 있는 사용자 데이터베이스와 데이터 사전을 관리하고 접근 <br>
      - 디스크에 저장된 데이터에 접근하는 것은 운영체제의 기본 기능이므로 저장 데이터 관리자는 운영체제의 도움을 받아 데이터베이스에 대한 접근을 수행 <br>
      > **데이터 사전이란**  <br>
      > 시스템 카탈로그라고도 하며 데이터베이스에 저장되는 데이터에 관한 정보를 저장함 <br>
      > 데이터에 대한 정보를 의미하기 때문에 메타 데이터라고도 함 <br>
3. 필수 기능
   - 데이터 정의 기능
      - 데이터의 논리적, 물리적 구조를 정의할 수 있어야함
   - 데이터 조작 기능
      - 사용자가 자연 언어에 가까운 수준으로 데이터를 검색, 변경, 삭제할 수 있어야함
      - 데이터의 접근 방법이 효율적이며 명확해짐
   - 데이터 제어 기능
      - 동시성 제어 기능 : 사용자가 동시에 데이터를 사용하고자 할 때 감시, 감독하는 기능이 있어야함
      - 보안 권한 기능: 데이터를 외부로부터 보호해야 하며 데이터의 사용 권한을 구분하여 사용할 수 있도록 해야함
      - 무결성 및 제약조건 유지 기능 : 데이터가 변경, 수정되는 과정에서 데이터의 정확성과 일관성이 유지되도록 해야함   
4. 특징
   - 데이터 무결성
     - 부적절한 자료가 입력되어 동일한 내용에 대하여 서로 다른 데이터가 저장되는 것을 허용하지 않는 성질
   - 데이터 일관성
     - 삽입, 삭제, 갱신, 생성 후에도 저장된 데이터가 변함없이 일정한 성질
   - 데이터 회복성
     - 장애가 발생하였을 시 원래 상태로 복구되어야 하는 성질
   - 데이터 보안성
      - 불법적인 노출, 변경, 손실로부터 보호되어야 하는 성질
   - 데이터 효율성
     - 응답 시간, 저장 공간 활용 등이 최적화되어 사용자, 소프트웨어, 요구 조건 등을 만족시켜야 하는 성질
5. 장단점
   - 장점
      - 데이터의 중복을 최소화함
      - 데이터를 많은 사용자가 공유할 수 있음
      - 데이터를 규칙에 맞게 표준화시켜 관리할 수 있음
      - 데이터의 보안과 무결성을 유지할 수 있음
      - 종합적인 데이터의 관리를 통해 데이터의 일관성을 유지 
   - 단점
      - 데이터의 규모가 크고, 복잡하여 구축 자체가 매우 어려움
      - 데이터 파괴에 대한 회복이 매우 어려움
      - 일정 부분에 문제가 발생하였을 때 전체 시스템에 영향을 주는 경우가 발생
      - 구축 비용 큼
6. 종류
      | DBMS | 제작사 | 작동 운영체제 | 기타 |
      |:---:|:---:|:---:|:---:|
      | MySQL | Oracle | Unix, Linux, Windows, Mac | 오픈 소스(무료), 상용 |
      | MariaDB | MariaDB | Unix, Linux, Windows | 오픈 소스(무료), MySQL 초기 개발자들이 독립해서 만듦 |
      | PostgreSQL | PostgreSQL | Unix, Linux, Windows, Mac | 오픈 소스(무료) |
      | Oracle | Oracle | Unix, Linux, Windows | 상용 시장 점유율 1위 |
      | SQL Server | Microsoft | Windows | 주로 중/대형급 시장에서 사용 |
      | DB2 | IBM | Unix, Linux, Windows | 메인프레임 시장 점유율 1위 |
      | Access | Microsoft | Windows | PC용 |
      | SQLite | SQLite | Android, IOS | 모바일 전용, 오픈 소스(무료) |

<br>

### 데이터베이스 관리 시스템의 발전 과정.
#### 데이터베이스 관리 시스템의 발전 과정.
1. 발전 과정 <br>
   | 구분 | 모델 | DMBS |
   |:---:|:---:|:---:|
   | 1세대 | 파일시스템 | ISAM <br> VSAM |
   | 2세대 | 계층형 DBMS (HDBMS) | IMS <br> System2000 |
   | 3세대 | 네트워크형 DBMS (NDBMS) | IDS <br> TOTAL <br> IDMS |
   | 4세대 | 관계형 DBMS (RDBMS) | Oracle <br> My-SQL <br> DB2 <br> SQL Server <br> Sybase |
   | 5세대 | 객체지향형 DBMS (ODBMS) | Object Store <br> UniSQL | <br>
   
   - 계층형 DBMS (HDBMS) <br>
     <img src = "https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EA%B3%84%EC%B8%B5%ED%98%95DBMS.png" width ="60%"> <br>
     - 처음으로 등장한 DBMS 개념으로 1960년대에 시작되었음
     - 각 계층은 트리 형태를 갖음
     - 처음 구성을 완료한 후에 이를 변경하기가 까다로움
     - 다른 구성원을 찾아가는 것이 비효율적
     - 지금은 사용하지 않는 형태 <br>
   - 네트워크형 DMBS (NDBMS)<br>
     <img src = "https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/%EB%A7%9D%ED%98%95DBMS.png" width ="60%"> <br>
     - 계층형 DBMS의 문제점을 개선하기 위해 1970년대에 등장
     - 하위에 있는 구성원들끼리도 연결된 유연한 구조
     - 망형 DBMS를 잘 활용하려면 프로그래머가 모든 구조를 이해해야만 프로그램 작성이 가능하다는 단점이 있음
     - 지금 거의 사용하지 않는 형태 <br>
   - 관계형 DBMS (RDBMS)<br>
     <img src = "https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/sql_table.png" width ="60%"> <br>
     - Relational DBMS
     - 대부분의 DMBS가 RDBMS 형태로 사용됨
     - 테이블로 이루어져있으며, 테이블은 열과 행으로 이루어져 있음 (테이블은 RDMBS의 최소 단위) <br>
   - 객체지향형 DBMS (ODBMS)<br>
     <img src = "https://prepinstadotcom.s3.ap-south-1.amazonaws.com/wp-content/uploads/2023/01/Object-Oriented-Database-Model-in-DBMS.webp" width ="60%"> <br>
     - 1980년대에 개발된 DBMS
     - 데이터를 객체의 형태로 표현하는 방식
     - 객체지향 프로그래밍 언어와 호환성이 높고 복잡한 데이터 타입을 지원함
     - 성능이 낮고 표준화가 부족하고 SQL과 호환되지 않음
   - No SQL <br>
     <img src = "https://learn.microsoft.com/ko-kr/dotnet/architecture/cloud-native/media/types-of-nosql-datastores.png" width ="60%"> <br>
     - Not Only SQL
     - 2000년대에 개발된 DBMS
     - 테이블 형태의 관계형 모델이 아닌 여러가지 모델로 데이터를 표현하는 방식
     - 어느 한가지 형태의 데이터베이스를 지칭하지 않고, RDBMS의 테이블 형태가 아닌 형태를 띈 DB를 총칭
     - RDMS보다 확장성과 가용성이 높고 유연한 스키마를 가짐
     - 대용량 데이터나 분산 처리에 매우 빠르게 대응 가능
     - 데이터의 일관성은 보장되지 않고 기존 SQL과 호환되지 않음
3. 언어 분류 <br>
   <img src = "https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/DBMS-%EC%A0%9C%ED%92%88.png" width ="60%"> <br>

<br>

> **출처** <br>
> <a href="https://dobby-the-house-elf.tistory.com/85">데이터베이스 기본 구조</a><br>
> <a href="https://hoyashu.tistory.com/6">컬럼과 필드 차이</a> <br>
> <a href="https://hongong.hanbit.co.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-databasedb-dbms-sql%EC%9D%98-%EA%B0%9C%EB%85%90/">DBMS 개념</a> <br>
> <a href="https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90">데이터베이스 개념, DBMS 발전 과정</a>
