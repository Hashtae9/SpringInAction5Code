# SQL

## 1. SQL 종류

### 	+ DDL(Data Definition Langauage)

+ CREATE : 객체(테이블, 뷰, 인덱스, 시퀸스)를 생성
+ DROP : 객체를 삭제
+ ALTER : 객체를 변경
+ TRUNCATE TABLE : 테이블에 있는 모든 데이터 삭제 
+ RENAME : 객체 이름을 변경



### + DML(Data Manipulation Langauage)

+ SELECT : 테이블이나 뷰에서 데이터 조회
+ INSERT : 데이터를 입력
+ UPDATE : 기존에 저장된 데이터를 수정
+ DELETE : 테이블에 있는 데이터를 삭제
+ MERGE : 조건에 따라 INSERT와 UPDATE를 수행



### + TCL(Transaction Control Langauge)

+ COMMIT : DML로 변경된 데이터를 DB에 적용
+ ROLLBACK : DML로 변경된 데이터를 변경 이전 상태로 되돌림



### + DCL(Data Control Langauge)

+ GRANT : 객체에 대한 권한을 할당
+ REVOKE : 객체에 할당된 권한을 회수



## 2. DDL

```sql
CREATE TABLE table_name( //CREATE TABLE 테이블 명(테이블 관련 정보)
	column_name1 datatype [NOT] NULL, //컬럼명 컬럼의데이터유형 NULL허용여부
	column_name2 datatype [NOT] NULL,
	...
	PRIMARY KEY( column_list) //PK 정의
	);
```

+ 테이블
  + 컬럼만 정의
    + 30byte를 넘지 않음
    + 언더스코어(_), 문자, 숫자를 사용할 수 있지만, 이름의 첫 문자는 반드시 문자로 시작



+ 컬럼의 데이터형

| 데이터 유형 | 데이터형         | 설명                                                         |
| ----------- | ---------------- | ------------------------------------------------------------ |
| 문자형      | CHAR(n)          | 고정 길이 문자, 최대 2000byte                                |
|             | VARCHAR2(n)      | 가변 길이 문자, 최대 4000byte                                |
| 숫자형      | NUMBER[(p, [s])] | p(1~38, 디폴트 값은 38)와 s(-84~127, 디폴트 값은 0)는 십진수 기준, 최대 22byte |
| 날짜형      | DATE             | BC 4712년 1월 1일부터 9999년 12월 31일까지 년, 월, 일, 시, 분, 초까지 입력 가능 |

+ []는 생략 가능



+ NULL

  > 데이터가 없음을 의미



+ PK(기본 키)

  > 테이블에서 유일한 값을 식별하는 역할
  >
  > 테이블 당 1개

  + Column_name1 VARCHAR2(10) NOT NULL **PRIMARY KEY**,
  + PRIMARY KEY(column1, ...)



#### 테이블 생성 예시

```sql
CREATE TABLE emp03
(
	emp_id NUMBER NOT NULL,
	emp_name VARCHAR2(100) NOT NULL,
	gender VARCHAR2(10) NULL,
	age NUMBER NULL,
	hire_date DATE NULL,
	etc VARCHAR2(300) NULL,
	PRIMARY KEY(emp_id)
);
```



## 3. 데이터 입력(INSERT)

```sql
INSERT INTO 테이블명(column1, column2, ...)
VALUES(값1, 값2, ...);
```

