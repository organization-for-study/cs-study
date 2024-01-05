# Database Data-Type

<br></br>

## 1. MYSQL vs ORACLE

<br></br>


### 1-1. 문자형 데이터 타입


- ORACLE

<img width="776" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/0aa6ae65-f525-4b68-8a23-c21472cc47ca">

- MYSQL

<img width="786" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/bc82eeae-8cd2-4e43-87df-e6bbd17da674">

<br></br>

1. CHAR 과 VARCHAR의 차이는 무엇일까?
2. VARCHAR 과 NVARCHAR의 차이는 무엇일까요?

<br></br>

### 1-2. 숫자형 데이터 타입


- ORACLE

<img width="781" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/e097a40b-727d-46b0-be3f-a0ce59dc5424">

- MYSQL

<img width="783" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/c5938c1a-2851-44d9-be4f-d0aae58cbc74">

<br></br>

1. 밑의 내용에서 다시 한번 나오겠지만 MYSQL의 TINYINT 데이터 타입은 boolean 데이터 타입을 대신하여 사용 됩니다.

<br></br>

### 1-3. 날짜형 데이터 타입

- ORACLE

<img width="769" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/dd8d9411-16f6-492d-a286-3e67be6f2fb9">

- MYSQL

<img width="786" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/db793eb5-fb7b-4ab9-81eb-d67c31bf5e71">

<br></br>

1. MYSQL에서 초단위 밑의 시간이 필요하다면 어떻게 사용해야 할까요?

<br></br>

### 1-4. 바이너리 데이터 타입

- ORACLE

<img width="781" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/7f9c8314-c851-4a17-b694-2d8ea31cc890">

- MYSQL

<img width="786" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/66c40972-a74f-401e-8008-12df9e5ea702">

<br></br>

## 2. DB별 boolean


### 2-1. PostgreSQL

<img width="789" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/fc0efe4e-7898-4dce-9ee9-edd6c898678b">

<img width="779" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/c9df0772-cfb5-4e63-a4eb-dfa7ec4179e8">

<br></br>

### 2-2. MYSQL

<img width="742" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/d2394e4d-e2b2-4825-829f-9a174c086c96">
<img width="780" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/13bc86db-faa4-483e-80e6-ff15a12bad06">
<img width="802" alt="image" src="https://github.com/BeenRepo/Reference/assets/140944695/2f97c755-8c39-45e7-9747-93ca54acf970">

### 2-3. ORACLE

1. NUMBER(1) 이나 NUMBER(0)으로 true,false 를 표현 합니다.
2. VARCHAR(1) 을 통하여 'Y' 혹은 'N' 으로 표현 합니다.

<br></br>


## 3. MYSQL - JSON Type

> JSON 타입은 COUNT(*) 등 직접 데이터에 접근하지 않아도 되지 않을 경우에는 BLOB 나 TEXT 데이터 타입을 사용하는 것과 성능이 크게 차이가 나지 않습니다.
> 하지만 데이터에 접근하고 클라이언트에 자료를 내려줘야 할때는 얘기가 달라 집니다.
> 다음 두 자료를 보며 JSON 데이터 타입 개념과 언제 사용하는 것이 좋을지 알아 봅시다.

#### 참고

MySQL JSON 이 BLOB, TEXT 에 비해 느린 이유 (JSON source 분석)

<https://wooseok-uzi.tistory.com/8>

MySQL JSON vs. TEXT

<https://medium.com/daangn/json-vs-text-c2c1448b8b1f>







































MySQL JSON 이 BLOB, TEXT 에 비해 느린 이유 (JSON source 분석)

<https://wooseok-uzi.tistory.com/8>

MySQL JSON vs. TEXT

<https://medium.com/daangn/json-vs-text-c2c1448b8b1f>

DB별 boolean type

<https://dev-j.tistory.com/15>

오라클 CHAR VARCHAR VARCHAR2

<https://infjin.tistory.com/130>

NVARCHAR 과 UTF-8

<https://sleepyeyes.tistory.com/94>

MYSQL 초단위 표현

<https://m.blog.naver.com/seuis398/220434359175>
