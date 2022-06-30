## HTTP 기본

### <SPAN STYLE ="COLOR:RED">1. HTTP(HyperText Transfer Protocol)</SPAN>

#### HTTP

> HTTP 메시지에 모든것을 전송

+ HTML, TEXT
+ IMAGE, 음성, 영상, 파일
+ JSON, XML(API)
+ 거의 모든 형태의 데이터 전송 가능
+ 서버 간에 데이터를 주고 받을 때도 대부분 HTTP 사용



**HTTP 역사**

+ HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더X 
+ HTTP/1.0 1996년: 메서드, 헤더 추가 
+ **HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전** 
  + RFC2068 (1997) -> RFC2616 (1999) -> RFC7230~7235 (2014) 
+ HTTP/2 2015년: 성능 개선 
+ HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선



**기반 프로토콜**

+ **TCP** : HTTP/1.1, HTTP/2
+ **UDP** : HTTP/3



#### **HTTP 특징**

+ 클라이언트 서버 구조
+ 무상태 프로토콜(스테이스리스), 비연결성
+ HTTP 메시지
+ 단순함, 확장 가능



### <SPAN STYLE ="COLOR:RED">2. 클라이언트 서버 구조</SPAN>

> 클라이언트와 서버를 분리
>
> > 비즈니스 로직과 데이터를 다 서버에서 관리
> >
> > UI, 사용성 등을 클라이언트가 관리
> >
> > > 클라이언트와 서버 각각 독립적으로 개발 가능

+ Request, Response 구조
+ 클라이언트는 서버에 요청을 보내고, 응답을 대기
+ 서버가 요청에 대한 결과를 만들어서 응답



### <SPAN STYLE ="COLOR:RED">3. Stateful, Stateless</SPAN>

#### 무상태 프로토콜(Stateless) <-> 상태 유지(stateful)

> 서버가 클라이언트의 (이전)상태를 보존x

+ 장점 : 서버 확장성이 높음(스케일 아웃)
+ 단점 : 클라이언트가 추가 데이터 전송

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615153703517.png" alt="image-20220615153703517" style="zoom:50%;" align="left"/>

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615153724535.png" alt="image-20220615153724535" style="zoom:50%;" align="left"/>



##### 상태 유지

> 중간에 다른 점원으로 바뀌면 안됨(중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야함)

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615154143229.png" alt="image-20220615154143229" style="zoom:90%;" align="left"/><img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615154233802.png" alt="image-20220615154233802" style="zoom:90%;" />



##### 무상태

> 중간에 다른 점원으로 바뀌어도 됨

+ 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입 가능
+ 무상태는 응답 서버를 쉽게 바꿀 수 있음 -> 무한한 서버 증설 가능
+ **필요한 정보**를 모두 담에서 요청을 보냄

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615154302070.png" alt="image-20220615154302070" style="zoom:80%;" /><img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615154320199.png" alt="image-20220615154320199" style="zoom:80%;" />

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615154516861.png" alt="image-20220615154516861" style="zoom:80%;" align="left"/>



#### Stateless 실무 한계

> 모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 존재
>
> 전송되는 데이터량이 Stateful과 비교하여 더 많음

+ 무상태
  + ex) 로그인이 필요없는 단순한 서비스
+ 상태 유지
  + ex) 로그인
+ 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
+ 일반적으로 브라우저 쿠기와 서버 세션등을 사용해서 상태유지
+ 상태유지는 최소한만 사용



### <SPAN STYLE ="COLOR:RED">4. 비 연결성(connectionless)</SPAN>

> HTTP는 기본이 연결을 유지하지 않는 모델

+ 일반적으로 초 단위의 이하의 빠른 속도로 응답
+ 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  + ex) 웹 브라우저에서 계속 연속해서 검색버튼을 누르지 않음
+ 서버 자원을 매우 효율적으로 사용  가능



##### 비 연결성의 한계

+ TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
+ 웹 브라우저로 사이트를 요청하면 HTML 뿐만아니라 자바스크립트, CSS, 추가 이미지 등 수많은 자원이 함께 다운
+ 지금은 HTTP 지속 연결(Pesistent Connections)로 문제 해결 (요청을 다 받고 종료)
+ HTTP/2, HTTP/3에서 더 많은 최적화

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615222317520.png" alt="image-20220615222317520" style="zoom:100%;" align="left" />![image-20220615222353012](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615222353012.png)







<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615175745690.png" alt="image-20220615175745690" style="zoom:80%;" align="left"/>

+ **연결을 유지하는 모델**
  + 클라이언트 1과 2가 서버와 한번 연결을 했다면 그 연결이 계속 유지됨
  + 연결을 유지하는 서버의 자원이 계속 소모가 됨(클라 1, 2가 놀고있어도 계속 유지를 해줘야 함)



<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615175930153.png" alt="image-20220615175930153" style="zoom:80%;" align="left"/>

+ **연결을 유지하지 않는 모델**
  + 서버는 요청에 대한 응답이 끝나면 연결을 끊어버림(최소한의 자원 사용)



### <SPAN STYLE ="COLOR:RED">5. HTTP 메시지</SPAN>

> 단순, 스펙도 읽어볼만 함
>
> HTTP 메세지도 단순



#### HTTP 메시지 구조

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615222836964.png" alt="image-20220615222836964" style="zoom:80%;" align="left"/>



#### <span style="color:blue">1. Start-line(시작라인)</span>

####  - 요청 메시지

+ **start-line** = **request-line**(요청은 request-line이 start-line, 응답은 status-line이 start-line)

+ **request-line** = **method** SP(공백) **request-target** SP **HTTP-version** CRLF(엔터) 

  + HTTP 메서드 (GET: 조회) 

    + 종류 : GET(리소스 조회), POST(요청 내역 처리), PUT, DELETE ...

    + 서버가 수행해야 할 동작 지정

  + 요청 대상 (/search?q=hello&hl=ko) 
    + absolute-path[?query] (절대경로[?쿼리])
    + 절대경로 = "/"로 시작하는 경로

  + HTTP Version



####  - 응답 메시지

+ **start-line** = **status-line** (요청은 request-line이 start-line, 응답은 status-line이 start-line)
+ **status-line** = **HTTP-version** SP **status-code** SP **reason-phrase** CRLF 



+ HTTP 버전 
+ HTTP 상태 코드: 요청 성공, 실패를 나타냄 
  + 200: 성공
  + 400: 클라이언트 요청 오류 
  + 500: 서버 내부 오류 
+ 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글



#### <span style="color:blue">2. HTTP 헤더</span>

+ header-field = **field-name** "**:**" OWS **field-value** OWS (OWS:띄어쓰기 허용) 

  > Host: www.google.com ( field-name과 : 사이에는 띄어쓰기 불가)

+ field-name은 대소문자 구문 없음

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615233822057.png" alt="image-20220615233822057" style="zoom:80%;" align="left"/>

+ 용도

  + HTTP 전송에 필요한 모든 부가정보
    + ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 ...
  + 표준 헤더가 너무 많음
    + https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
  + 필요시 임의의 헤더 추가 가능
    + helloworld: hihi

  

#### <span style="color:blue">3. HTTP 메시지 바디</span>

+ 용도
  + 실제 전송할 데이터
  + HTML 문서, 이미지, 영상 JSON 등 byte로 표현할 수 있는 모든 데이터 전송 가능

---------------

*reference*

https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#curriculum