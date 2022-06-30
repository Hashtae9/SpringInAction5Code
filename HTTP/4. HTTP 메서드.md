## HTTP 메서드

### <span style ="color:red"> 1. HTTP API</span>

#### 	회원 정보 관리 API 만들기

> 중요한 것은 **"리소스 식별"**



+ **API URI 고민**

##### 리소스 란?

> 회원을 등록하고 수정하고 조회하는 것이 아닌 회원이라는 개념 그 자체
>
> <span style="color:red">**URI는 리소스만 식별**</span>

+ 리소스와 해당 리소스를 대상으로 하는 행위를 분리
  + 리소스 : 회원
  + 행위 : 조회, 등록, 삭제, 변경
+ 리소스는 **명사** 행위는 **동사**



##### 리소스 식별

> 회원(리소스)를 등록하고 수정하고 조회하는 것을 모두 배제
>
> 회원이라는 리소스만 식별하면 됨 -> **회원 리소스를 URI에 매핑**



**리소스 식별** 예시-> 행위는 따로 분리

+ 회원 목록 조회 /members 
+ 회원 조회 /members/{id} 
+ 회원 등록 /members/{id} 
+ 회원 수정 /members/{id} 
+ 회원 삭제 /members/{id} 
+ 참고: 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권장(member -> members)



**행위** 구분

> **HTTP 메서드를 통해 해결**



### <span style ="color:red"> 2. HTTP 메서드 - GET, POST</span>

### HTTP 메서드

> 클라이언트가 서버에게 무언가 요청을 할 때 기대하는 행동

#### 주요 메서드

+ GET: 리소스 조회 
+ POST: 요청 데이터 처리, 주로 등록에 사용 
+ PUT: 리소스를 대체, 해당 리소스가 없으면 생성 
+ PATCH: 리소스 부분 변경 
+ DELETE: 리소스 삭제



#### 기타 메서드

+ HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환 
+ OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용) 
+ CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정 
+ TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행



#### GET

> <span style = "color:black">**리소스 조회**</span>

+ 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
+ 메시지 바디를 사용해서 데이터를 전달 가능하지만 지원하지 않는 곳이 많아서 권장x

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616094135863.png" alt="image-20220616094135863" style="zoom:100%;" align="left"/>![image-20220616094207359](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616094207359.png)



#### POST

> <span style="color:black">**요청 데이터 처리**</span>

+ 메시지 바디를 통해 서버로 요청 데이터 전달
+ 서버는 요청 데이터를 처리
  + 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행
+ 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용



**POST 기능**

1. **새 리소스 생성(등록)**  

   + 서버가 아직 식별하지 않은 새 리소스 생성 

2. **요청 데이터 처리**

   + 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우 
     + 예) 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우 

   + POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음
     + 예) POST /orders/{orderId}/start-delivery (컨트롤 URI) 

3. **다른 메서드로 처리하기 애매한 경우** 

   + 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우 
   + 애매하면 POST



![image-20220616094617434](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616094617434.png)![image-20220616094634568](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616094634568.png)

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616094735314.png" alt="image-20220616094735314" style="zoom:100%;" align="left"/>



#### POST

+ **스펙**: POST 메서드는 **대상 리소스가 리소스의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청**합니다. (구글 번역) 
+ 예를 들어 POST는 다음과 같은 기능에 사용됩니다. 
  + HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공 
    + 예) HTML FORM에 입력한 정보로 회원 가입, 주문 등에서 사용 
  + 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시 
    + 예) 게시판 글쓰기, 댓글 달기 
  + 서버가 아직 식별하지 않은 새 리소스 생성 
    + 예) 신규 주문 생성 
  + 기존 자원에 데이터 추가 
    + 예) 한 문서 끝에 내용 추가하기 
+ 정리: 이 리소스 URI에 P**OST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함** -> 정해진 것이 없음



### <span style ="color:red"> 3. HTTP 메서드 - PUT, PATCH, DELETE</span>

#### PUT

+ 리소스 대체(덮어쓰기)
  + 리소스가 있으면 대체
  + 리소스가 없으면 생성

+ **클라이언트가 리소스를 식별**
  + 클라이언트가 **리소스 위치를 알고 URI 지정**
  + POST와의 차이점



**리소스가 있는 경우**

![image-20220616103751115](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103751115.png)![image-20220616103804896](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103804896.png)



**리소스가 없는 경우**

![image-20220616103837511](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103837511.png)![image-20220616103849158](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103849158.png)





**PUT 주의점**

+ PUT은 이전의 리소스 자체를 삭제해버리고 대체(부분수정이 불가능)

![image-20220616103934998](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103934998.png)![image-20220616103948196](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616103948196.png)



#### PATCH

> <SPAN STYLE = "COLOR:BLACK">**리소스 부분 변경**</SPAN>

![image-20220616104143182](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616104143182.png)![image-20220616104155946](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616104155946.png)



#### DELETE

> <SPAN STYLE = "COLOR:BLACK">**리소스 제거**</SPAN>

![image-20220616104234325](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616104234325.png)![image-20220616104242578](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616104242578.png)





### <span style ="color:red"> 4. HTTP 메서드 속성</span>

+ 안전(Safe Methods) 
  + 호출해도 리소스를 변경하지 않음
    + GET은 단순한 조회이기에 안전
    + POST, PUT, DELETE,PATCH는 호출했을때 변경이 일어나기에 안전하지 않음
  + GET을 요청을 계속해서 로그가 쌓여서 장애 발생? -> 안전은 해당 리소스만 고려
+ 멱등(Idempotent Methods) 
  + f(f(x))=f(x)
  + 한번호출하던 두번 호출하던 100번 호출하든 결과가 똑같음
  + 멱등 메서드
    + GET : 한번 조회하든, 두번 조회하든 같은 결과가 조회됨
    + PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 동일(덮어쓰기한 결과가 동일하기 때문)
    + DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 동일
    + <SPAN STYLE = "COLOR:RED">POST</SPAN> : 멱등X, 두번 호출하면 같은 결제가 중복해서 발생 가능
  + 활용
    + 자동복구 메커니즘
    + 서버가 TIMEOUT 등으로 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가?의 판단 근거
  + 재요청 중간에 다른곳에서 리소스를 변경한다면?
    + 사용자1: GET -> username:A, age:20 
    + 사용자2: PUT -> username:A, age:30 
    + 사용자1: GET -> username:A, age:30 -> 사용자2의 영향으로 바뀐 데이터 조회 
    + 결론 : **멱등**은 **외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지는 않는다**
+ 캐시가능(Cacheable Methods)
  + 응답 결과 리소스를 캐시해서 사용해도 되는가?
  + GET, HEAD, POST, PATCH 캐시 가능
  + 실제로는 GET, HEAD 정도만 캐시로 사용
    + POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음



<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220616114919712.png" alt="image-20220616114919712" style="zoom:80%;" align="left"/>





--------------

*reference*