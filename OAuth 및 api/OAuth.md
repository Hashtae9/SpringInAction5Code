# OAuth

> 사용자의 인증 정보를 내 서비스에 넘기지 않고, **Access Token**을 이용해서 사용자 인증이 필요한 API에 접근할 수 있게 해주는 기술



### **OAuth 용어**

**1) Resource Owner**

\- 내 서비스를 이용하는 사용자
\- 자원의 소유자

**2) Client**

\- 내 서비스

**3) Resource Server**

\- 내 서비스와 연동되는 서비스
\- OAuth 서비스를 제공하고 자원을 관리하는 서버

**4) Authorization Server**

\- Token을 발급해주고, 권한을 관리하는 서버

**5) Access Token**

\- Authorization Server로 부터 발급 받은 Token



### **3. OAuth의 동작**

**1) OAuth 등록**

먼저, Client가 OAuth를 사용하기 위해 Resource Server(Google Cloud Platfrom과 같은)에게 승인을 받야아 한다.
이 과정은 연동할 서비스의 개발자 페이지에서 수행할 수 있다.
예) facebook for developers, Google Cloud Platform

등록 정보는 서비스마다 다르지만 공통적으로 가지는 부분은 다음과 같다.
**(1) Client ID : Resource Server가 내 서비스를 식별하는 ID**
**(2) Client Secret : 비밀번호**
**(3) Authorized Redirect URLs : Resource Server(google)가 Resource Owner(사용자)에게 Authorization Code를 포함하여 보내는 URL로, Resource Owner은 해당 주소로 이동하게 된다.**

OAuth 등록을 성공적으로 마치면, Resource Server와 Client는 저 3개의 정보들을 서로 알 수 있게 된다.



**2) Resource Owner의 승인**



![img](https://blog.kakaocdn.net/dn/Xvzqf/btq9Ah0qEwj/dFjZ7xpnjBkZbyjkWfekBK/img.png)



Client는 Resource Owner가 Resource Server의 서비스를 이용할 수 있게 하기 위한 링크를 제공해 주어야 한다.
그리고 Resource Owner는 해당 링크를 누름으로써 Resource Server에 접근하겠다는 의사표시를 할 수 있다.

해당 링크는 id, scope(사용할 기능의 범위), redirect_uri 등을 포함하여 Resource Server로 전송할 수 있는 주소로 설정하면 된다. 아래 이미지가 이해에 도움이 될 것이다.



![img](https://blog.kakaocdn.net/dn/tMbrT/btq9Gl1ygBH/mETZ4kWtkE7y87WZC3o9xK/img.png)



Resource Owner(사용자)가 해당 링크로 이동하게 되면 Resource Server은 URL에 포함된 Client ID가 자신의 서버에 등록된 Client ID 리스트에 속해 있는지 확인하고, 이 Client ID에 해당되는 Redirect URL까지 일치하는지 확인한다.
일치하다면, Resource Server(google)은 URL에 포함된 Scope의 권한들을 Client(웹)에게 부여 할 것인지 Resource Owner(사용자)에게 동의 의사를 묻는다.



![img](https://blog.kakaocdn.net/dn/ckplGs/btq9GtyEX8c/93VCRXLFm7FscfL8x0bg31/img.png)




Resource Owner가 수락했다면, Resource Server은 이 Resource Owner의 ID와 수락받은 Scope를 같이 저장해둔다.



 

**3) Resource Server의 승인**

Resource Server은 Client(웹)가 Resource Owner(사용자)으로부터 Scope의 권한들을 무사히 부여 받았다는 것을 확인해야 한다.
이를 위해서, Resource Server은 Resource Owner에게 Authorization code를 포함한 Redirect URI을 전송한다.
그리고 이 Redirect URI는 **Resource Owner(google)을 걸쳐 Client(내 서비스)에게 전송**되어 Client는 Authorization code값을 받게 된다.
**Authorization code는 Access Token을 받기 위해 필요한 임시 비밀번호**이다.

그 다음에 Client는 Resource Server로 Grant type, Authorization code, Redirect URI, Client ID, Client secret을 모두 전송하고, Resource Server은 이 정보들이 모두 맞는지 확인한 뒤에 Authorization Server을 통해 Access Token을 발급 해준다.



![img](https://blog.kakaocdn.net/dn/cHHxoX/btq9DvkreA5/9O2YVdqmCFYI2uq2y45Xh1/img.png)



 

 

**4) Access Token 발급★**

Client가 Access Token을 성공적으로 발급받았다면 Authorization code는 더 이상 필요가 없기 때문에 삭제되고, Resource Owner는 안전하게 Resource Server와 상호작용 할 수 있게 된다.

 

 

**5) API 사용**

이제 모든 인증 절차는 끝났다.

Client는 자신의 서비스에 Resource Server(google)가 제공하는 기능들을 포함시켜야 하는데, 이 때 사용하는 것이 API(Application Programming Interface)다.

API는 특정 응용 프로그램을 사용하기 위한 약속으로, Client는 Resource Server에서 제공하는 API 문서들을 참고하여 API들의 사용법을 파악하고 자신의 서비스에 적용하면 된다.

 



### **4. Refresh Token**

만료된 Access Token을 갱신하기 위한 토큰이다.

Access Token을 발급해줄 때 Refresh Token을 같이 발급해주는 곳도 있고, Refresh Token을 사용하지 않는 곳도 있다.

Refresh Token 사용법은 서비스마다 다르기 때문에 관련 문서를 찾아보면 된다.



# 실제 토큰 받아오는 과정

1. **사용자 인증 정보 -> 사용자 인증정보 만들기**

![image-20220702012024423](https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012024423.png?)

![image-20220702012055488](https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012055488.png?)

2. **OAuth 동의 화면 구성**

![image-20220702012254796](https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012254796.png?)

3. **OAuth 클라이언트 ID, 비밀번호 받기**

![image-20220702012138803](https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012138803.png?)

4. 라이브러리 -> 사용할 서비스 선택

![image-20220702012538755](https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012538755.png)



# API 문서 확인법

### 1. HTTP request

> 요청 URL이다. 이 URL뒤에 요청 파라미터들을 넣어야 완전한 요청 URI가 되는 것이다.

<img src="https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702012738346.png?" alt="image-20220702012738346" style="zoom:80%;" align="left"/>



### 2. Parameters

> 요청 파라미터들이다.
>
> 여기에는 나와있지 않지만, 사용자를 식별하기 위한 **access_token값도 파라미터에 반드시 포함**시켜야 한다. 지금 당장은 access_token값을 알 수 없다.
>
> **Optional은 선택 사항이기 때문에 생략이 가능**하며, **생략했을 때의 기본값을 default**라고 한다는 사실도 반드시 기억해두자.

<img src="https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702013047146.png?" alt="image-20220702013047146" style="zoom:80%;" align="left"/>



### 3. Authorization

> Scope는 사용할 기능에 대한 인증 양식이다.
>
> 최소 하나 이상의 Scope의 양식을 사용해야 한다고 명시되어있다.

<img src="https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702013221357.png?" style="zoom:100%;" align="left"/>

<img src="https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702013623959.png?" alt="image-20220702013623959" style="zoom:120%;" align="left"/>



### 4. Response

> 응답 메세지의 구성에 대한 설명이다.
>
> 각 JSON 오브젝트의 데이터가 무슨 의미를 가지는지 설명한다.

<img src="https://raw.githubusercontent.com/Hashtae9/image_repo/main/img/image-20220702013855408.png" alt="image-20220702013855408" style="zoom:100%;" align="left"/>

### 5. 사용 예시 및 사용해보기

![image](https://user-images.githubusercontent.com/101400894/177009138-430b1ba5-0585-4595-9878-a924aa71dacf.png)

![image](https://user-images.githubusercontent.com/101400894/177009117-bbf81b78-1efa-40e9-914d-09ffc50f74c8.png)





# Authorization code 받기

### <span style = "color:red">1.Resource Owner의 승인</span>

> Client는 Resource Owner가 Resource Server의 서비스를 이용할 수 있게 하기 위한 링크를 제공해 주어야 하고, Resource Owner는 해당 링크를 누름으로써 Resource Server에 접근하겠다는 의사표시를 할 수 있다.
>
> 그리고 Google Identity Platform 문서에서는 Resource Owner가 요청해야 하는 URI 양식을 안내해준다.

#### 1-1. 요청 URI 확인

<img src="https://user-images.githubusercontent.com/101400894/177009084-a5e0bc35-5cb3-4b80-922f-57b7056cecea.png" alt="image" style="zoom:110%;" align="left"/>

(1) 필수 파라미터

\- client_id : API 콘솔 인증 정보 페이지에서 찾을 수 있음

\- redirect_uri : API 콘솔 인증 정보 페이지에서 승인된 리디렉션 URI

\- response_type : Google OAuth 2.0 endpoint에서 authorization code를 반환할지에 대한 여부. 웹 서버 애플리케이션 환경에서는 code라고 설정하면 됨

\- scope : 접근 가능한 리소스 범위, API 문서에서 확인 가능



(2) 권장 파라미터

\- access_type : 사용자가 브라우저에 없을 때에도 refresh token을 발급 받을 수 있는지에 대한 여부 (online/offline 택1) 

\- state : 인증 요청과 인증 서버의 응답간의 상태를 유지하기 위한 string



(3) 선택 파라미터

\- include_granted_scope : true로 설정하고 요청이 허가되면, 새 access token은 이전에 허가 받았던 access 범위도 포함함(true/false 택1)

\- login_hint : Google 인증 서버에 로그인 힌트를 제공

\- prompt : 안내 메세지 관련 파라미터 (none/consent/select_account 택1)



#### 1-2. 리디렉션 URI 등록

> Authorization code를 받을 페이지를 생성하고, 이 페이지의 경로(redirect_uri)를 등록해야 한다.

![image](https://user-images.githubusercontent.com/101400894/177009069-e8143eb4-a9f0-4c09-a157-0b605bd9cb67.png)



#### 1-3. 요청 URI 작성 예시

```http
https://accounts.google.com/o/oauth2/v2/auth
?scope=https://www.googleapis.com/auth/calendar https://www.googleapis.com/auth/calendar.readonly 
&access_type=offline
&include_granted_scopes=true
&response_type=code
&state=state_parameter_passthrough_value
&redirect_uri=http://localhost:8080/receiveAC
&client_id=499264841555-1kqekv3g1khc8jn5848cn1i8pcp12t29.apps.googleusercontent.com
```



<goocal.mustache>

```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Google Calendar</title>
    </head>
    <body>
        <a href="http://accounts.google.com/o/oauth2/v2/auth?
 scope=https%3A//www.googleapis.com/auth/drive.metadata.readonly&
 access_type=offline&
 include_granted_scopes=true&
 response_type=code&
 state=state_parameter_passthrough_value&
 redirect_uri=http://localhost:8080/receiveAC&
 client_id=360391961315-h0nbrrpq59guev2tfnn7acas331odk8g.apps.googleusercontent.com">
            요청 URI
        </a>
    </body>
</html>
```



### <span style = "color:red">2. Access Token 받기</span>

#### 2-1. 요청 URI 양식

>Client는 Authorization Server에게 access token을 받기 위해, 최종 인증을 위한 요청 URI을 작성해야 한다. 공식 문서에서 양식을 살펴보자.
>
>먼저, endpoint는 "https://oauth2.googleapis.com/token"이라고 명시되어 있다.
>
>그리고 client_id, client_secret, code, grant_type, redirect_uri은 모두 **필수 파라미터**이다.

![image](https://user-images.githubusercontent.com/101400894/177009054-406db491-8acb-4c49-8735-00a6c975fd78.png)



\- client_id : API 콘솔 인증 정보 페이지에서 찾을 수 있음

\- client_secret : API 콘솔 인증 정보 페이지에서 찾을 수 있음. 노출되어서는 안되는 비밀번호.

\- code : 발급 받은 authorization code

\- grant_type : 승인 type. authorization_code라고 설정하면 됨

\- redirect_uri : API 콘솔 인증 정보 페이지에서 승인된 리디렉션 URI. Resource Owner가 요청했을 때의 redirect_uri 값과 동일하게 설정하면 된다.



#### 2-2. 요청 URI 작성

> 요청 URI로 요청을 보낼 수 있도록 receiveAC.mustache를 수정했다.
>
> client_secret은 기밀정보이므로 POST 방식으로 전송하도록 했다. 

```http
<html>
<head>
    <meta charset="UTF-8">
    <title>Google Calender</title>
</head>
<body>
    <form id="acform" action="https://oauth2.googleapis.com/token" method="post"
          enctype="application/x-www-form-urlencoded">
        <input type="hidden" name="code" value="{{code}}">
        <input type="hidden" name="client_id" value="360391961315-h0nbrrpq59guev2tfnn7acas331odk8g.apps.googleusercontent.com">
        <input type="hidden" name="client_secret" value="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">
        <input type="hidden" name="redirect_uri" value="http://localhost:8080/receiveAC">
        <input type="hidden" name="grant_type" value="authorization_code">
    </form>
    <script type="text/javascript">
        this.document.getElementById("acform").submit();
    </script>
</body>
</html>
```

<소스 코드 설명>

**1) {{code}}**

전송 받은 code(authorization code)를 자동으로 적용 할 수 있게 하기 위해, mustache template 엔진에서 제공하는 기능을 이용한 것이다. code를 변수로 사용할 수 있게 중괄호 2개로 감싸줬다.

 

이 방법을 사용하려면 전송 받은 code(authorization code)값을 활용할 수 있도록 하는 사전 작업이 필요하다.

필자는 SpringBoot의 Controller를 이용하여, 전송 받은 code값을 receiveAC.mustache로 전달했다.

```java
	@GetMapping("/receiveAC")
    public String receiveAC(@RequestParam("code") String code, Model model){
        model.addAttribute("code",code);
        return "receiveAC";
    }
```



**2) redirect_uri**

redirect_uri의 값을 encode 하지 않고 그대로 붙여 넣은 이유는 form 태그의 enctype 속성값을 "application/x-www-form-urlencoded"로 설정했기 때문이다.

 

**3) <script>**

form을 submit하는 과정을 자동화하기 위하여 javascript를 사용했다.



### **3. 요청 URI 전송**

> SpringBoot를 재시작하고, 프로젝트 내에서 구글 로그인을 수행했더니 성공적으로 access_token값과 refresh_token값을 받을 수 있었다.

![image](https://user-images.githubusercontent.com/101400894/177009037-e0b45922-8fd4-443a-b94e-a7a0619be8d3.png)

