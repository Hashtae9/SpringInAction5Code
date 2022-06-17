## HTTP 메서드 활용

### <SPAN STYLE = "COLOR:RED"> 1. 클라이언트에서 서버로 데이터 전송</SPAN>

#### 전달방식

1.  **쿼리 파라미터**를 통한 데이터 전송
   + GET
   + 주로 정렬 필터(검색어)
2.  **메시지 바디**를 통한 데이터 전송
   + POST, PUT, PATCH
   + 회원가입, 상품 주문, 리소스 등록, 리소스 변경



#### 4가지 상황

1. **정적 데이터 조회**
   + 이미지, 정적 텍스트 문서
   + 조회는 GET 사용
   + 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617140951992.png" alt="image-20220617140951992" style="zoom:80%;" align="left"/>

-----------

2. **동적 데이터 조회**

   + 주로 검색, 게시판 목록에서 정렬 필터(검색어) 

   + 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용

   + 조회는 GET 사용

   + GET은 쿼리 파라미터 사용해서 데이터 전달



<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617141130596.png" alt="image-20220617141130596" style="zoom:80%;" align="left"/>

---------------------------------------

3.  **HTML Form**을 통한 데이터 전송 

   + HTML Form submit 시 POST 전송
     + 회원 가입, 상품 주문, 데이터 변경

   + Content-Type : application/x-www-form-urlencoded 사용
     + form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 방식)
     + 전송 데이터를 url encoding 처리
       + ex) abc김 -> abc%EA%B9%80

   + HTML Form은 GET 전송 가능

   + Content-Type : multipart/form-data
     + 파일 업로드 같은 바이너리 데이터 전송 시 사용
     + 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)

   + HTML Form 전송은 GET, POST만 지원



<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617141429838.png" alt="image-20220617141429838" style="zoom:80%;" /><img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617141541492.png" alt="image-20220617141541492" style="zoom:80%;" /><img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617142241311.png" alt="image-20220617142241311" style="zoom:80%;" /><img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617142353415.png" alt="image-20220617142353415" style="zoom:80%;" />

-------------------------------

4.  HTTP API를 통한 데이터 전송 

   + 회원 가입, 상품 주문, 데이터 변경 

   + 서버 to 서버
     + 백엔드 시스템 통신
   + 앱 클라이언트
     + 아이폰, 안드로이드
   + 웹 클라이언트(Ajax)
     + HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
     + ex)React, VueJs 같은 웹 클라이언트와 API 통신
   + POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송
   + GET : 조회, 쿼리 파라미터로 데이터 전달
   + Content-Type : application/json을 주로 사용(사실상 표준)
     + TEXT, XML, JSON 등등

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220617143822887.png" alt="image-20220617143822887" style="zoom:80%;" align="left"/>

--------------------------

### <SPAN STYLE = "COLOR:RED"> 2. HTTP API 설계 예시</SPAN>

> 회원관리용 API를 계발한다고 가정

#### 리소스 원형

> 리소스 원형을 REST API에 URI를 디자인할 때 쓰는 모든 Resource들을 지칭하는 말인데, 도큐먼트, 컬렉션, 스토어, 컨트롤러로 구성

+ 도큐먼트 

  > 데이터베이스의 row단위 또는 리스트 컬렉션의 하나의 객체단위로 단일 정보를 포함하고 있는 데이터라고 생각하면 쉽다. 

+ 컬렉션  

  > 도큐먼트들의 집합
  >
  > 관리 주체는 서버이며 컬렉션은 클라이언트에서 새로운 리소스를 제안해서 올릴 수 있으나 결정은 서버가 한다. 

+ 스토어   

  > 컬렉션과 같이 도큐먼트들의 집합이며 관리 주체는 클라이언트
  >
  > 클라이언트는 기존의 리소스를 스토어 추가하거나 삭제 변경할 수 있지만 새로운 리소스를 생성할 수 없다. 

+ 컨트롤러 

  > 컬렉션, 스토어의 메서드의 기능
  >
  > HttpMethod(CRUD)를 제외한 특정한 기능을 요구할 때 사용된다.



### 회원 관리 시스템

+ **HTTP API - 컬렉션** 
  + **POST** 기반 등록 
  + 예) 회원 관리 API 제공 
+ **HTTP API - 스토어** 
  + **PUT** 기반 등록 
  + 예) 정적 컨텐츠 관리, 원격 파일 관리 
+ **HTML FORM 사용** 
  + 웹 페이지 회원 관리 
  + GET, POST만 지원



### <span style="color:blue">HTTP API - 컬렉션</span>

#### API 설계 - POST 기반 등록

+ 회원 목록 /members   **-> GET**
+ 회원 등록 /members   **-> POST**                         // 이런 /members를 컬렉션이라 함.
+ 회원 조회 /members/{id}   **-> GET**
+ 회원 수정 /members/{id}   **-> PATCH, PUT, POST**
+ 회원 삭제 /members/{id}   **-> DELETE**



#### POST - 신규 자원 등록 특징

+ 클라이언트는 **등록될 리소스의 URI를 모른다.** 
  + 회원 등록 /members -> POST 
  + POST /members 
+ 서버가 새로 등록된 리소스 URI를 생성해준다.
  + HTTP/1.1 201 Created 
    Location: **/members/100**  
+ **컬렉션(Collection)** 
  + **서버가 관리하는 리소스 디렉토리** 
  + 서버가 리소스의 URI를 생성하고 관리
  + 여기서 컬렉션은 /members



### <span style="color:blue">HTTP API - 스토어</span>

#### API 설계 - PUT 기반 등록

+ 파일 목록 /files **-> GET** 
+ 파일 조회 /files/{filename} **-> GET** 
+ 파일 등록 /files/{filename} **-> PUT** 
+ 파일 삭제 /files/{filename} **-> DELETE**  
+ 파일 대량 등록 /files **-> POST**



#### PUT - 신규 자원 등록 특징

+ 클라이언트가 리소스 URI를 알고 있어야 한다. 
  + 파일 등록 /files/{filename} -> PUT 
  + PUT /files/star.jpg 
+ 클라이언트가 직접 리소스의 URI를 지정한다. 
+ **스토어(Store)** 
  + **클라이언트가 관리하는 리소스 저장소** 
  + 클라이언트가 리소스의 URI를 알고 관리 
  + 여기서 스토어는 /files



### <span style="color:blue">HTML FORM</span>

+ HTML FORM은 GET, POST만 지원 
+ AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API 참고 
+ 여기서는 순수 HTML, HTML FORM 이야기 
+ GET, POST만 지원하므로 제약이 있음
+ **컨트롤 URI**  
  + GET, POST만 지원하므로 제약이 있음 
  + 이런 제약을 해결하기 위해 **동사로 된 리소스 경로 사용** 
  + POST의 /new, /edit, /delete가 컨트롤 URI 
  + **HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)**



#### HTML Form 사용

+ 회원 목록       /members **-> GET** 
+ 회원 등록 폼  /members/new **-> GET** 
+ 회원 등록       /members/new, /members **-> POST** 
+ 회원 조회       /members/{id} **-> GET** 
+ 회원 수정 폼  /members/{id}/edit **-> GET** 
+ 회원 수정       /members/{id}/edit, /members/{id} **-> POST** 
+ 회원 삭제       /members/{id}/delete **-> POST**



#### <span style="color:red">참고하면 좋은 URI 설계 개념</span>

+ 문서(document)  
  + 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row) 
  + 예) /members/100, /files/star.jpg 
+ 컬렉션(collection)  
  + 서버가 관리하는 리소스 디렉터리 
  + 서버가 리소스의 URI를 생성하고 관리 
  + 예) /members 
+ 스토어(store)  
  + 클라이언트가 관리하는 자원 저장소 
  + 클라이언트가 리소스의 URI를 알고 관리 
  + 예) /files 
+ 컨트롤러(controller), 컨트롤 URI  
  + 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행 
  + 동사를 직접 사용 
  + 예) /members/{id}/delete



--------------

*reference*

[REST Resource Naming Guide (restfulapi.net)](https://restfulapi.net/resource-naming/)

https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#curriculum