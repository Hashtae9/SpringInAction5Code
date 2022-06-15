## URI와 앱 브라우저 요청 흐름

### <span style="color:red">1. URI(Uniform Resource Identifier)</span>

#### URI

> Uniform : 리소스 식별하는 통일된 방식
>
> Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
>
> Indentifier : 다른 항목과 구분하는데 필요한 정보
>
> > 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있음

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615120435441.png" alt="image-20220615120435441" style="zoom:55%;" align="left"/>



##### URL(Uniform Resource Locator)

> 리소스가 있는 위치를 지정

##### URN(Uniform Resource Name)

> 리소스에 이름을 부여

+ 비교
  + URL(위치)는 변할 수 있지만, URN(이름)은 변하지 않음
  + URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음



<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615120709642.png" alt="image-20220615120709642" style="zoom:50%;" align="left"/>



#### URL 분석

https://www.google.com/search?q=hello&hl=ko

+ **전체 문법**

```
scheme://[userinfo@]host[:port][/path][?query][#fragment]
https://www.google.com:443/search?q=hello&hl=ko
```

+ 프로토콜(https) 
+ 호스트명(www.google.com) 
+ 포트 번호(443)  
+ 패스(/search) 
+ 쿼리 파라미터(q=hello&hl=ko)



###### scheme

> 주로 프로토콜 사용
>
> > 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙  (ex)http(80포트), https(443포트), ftp 등 

+ 포트는 생략 가능
+ https는 http에 보안 추가(HTTP Secure)



###### userinfo

> URL에 사용자정보를 포함해서 인증(거의 사용 X)



###### host

> 호스트명
>
> 도메인명 또는 IP 주소를 직접 사용 가능



###### port

> 포트(접속 포트)
>
> 일반적으로 생략, 생략시 http는 80, https는 443



###### path

> 리소스 경로(path), 계층적 구조

ex)

+ /home/file1.jpg
+ /members
+ /members/100
+ /items/iphone12



###### query

> key=value 형태
>
> ?로 시작, &로 추가 가능
>
> > ?keyA=valeuA&keyB=valueB

+ query parameter, query string등으로 불림
+ 웹 서버에 제공하는 파라미터, 문자 형태



###### fragment

> html 내부 북마크 등에 사용
>
> 서버에 전송하는 정보 아님

```
scheme://[userinfo@]host[:port][/path][?query][#fragment]
https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-introducing-spring-boo
```





### <span style="color:red">2. 웹 브라우저 요청 흐름</span>

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615144610652.png" alt="image-20220615144610652" style="zoom:50%;" align="left"/>

+ 동작 순서
  1. URL 입력(요청)
  2. DNS 조회 (상대 IP 찾기)
  3. HTTP 요청 메시지 생성
  4. HTTP 메시지 전송(사진 참조)

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615144843984.png" alt="image-20220615144843984" style="zoom:53%;" align="left"/>

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615145105872.png" alt="image-20220615145105872" style="zoom:50%;" align="left"/>

		5. HTTP 응답 메시지 전송

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615145146168.png" alt="image-20220615145146168" style="zoom:50%;" align="left"/>

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220615145219082.png" alt="image-20220615145219082" style="zoom:50%;" ALIGN="LEFT"/>









