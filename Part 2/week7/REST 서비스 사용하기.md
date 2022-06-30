# REST 서비스 사용하기

## 	목표

1. **REST API 클라이언트를 작성하고 사용하는 방법 이해하기**

2. **REST API 클라이언트가 추가된 타코 클라우드 애플리케이션을 빌드하고 실행**



## 	사용할 방법

1. RestTemplate : 스프링 프레임워크에서 제공하는 간단하고 동기화된 REST 클라이언트
2. Traverson : 스프링 HATEOAS에서 제공하는 하이퍼링크를 인식하는 동기화 REST 클라이언트
   + 같은 이름의 자바 스크립트 라이브러리로부터 비롯됨
3. WebClient : 스프링 5에서 소개된 반응형 비동기 REST 클라이언트



## 1. RestTemplate

### RestTemplate 이란?

> 스프링에서 제공하는 http 통신에 유용하게 쓸 수 있는 템플릿
>
> HTTP 서버와의 통신을 단순화하고 RESTful 원칙을 지킴



### RestTemplate 메서드

> RestTemplate이 정의하는 고유한 작업을 수행하는 12개의 메서드

|        메서드        |  HTTP   | 설명                                                         |
| :------------------: | :-----: | ------------------------------------------------------------ |
|        delete        | DELETE  | 지정된 URL의 리소스에 HTTP DELETE 요청을 수행                |
|    exchange(...)     |   any   | 지정된 HTTP 메서드를 URL에 대해 실행<br />응답 몸체와 연결되는 객체를 포함하는 ResponseEntity를 반환<br />HTTP 헤더를 새로 만들 수 있고 어떤 HTTP 메서드도 사용가능 |
|     execute(...)     |   any   | 지정된 HTTP 메서드를 URL에 대해 실행<br />응답 몸체와 연결되는 객체를 반환<br />Request/Response 콜백을 수정 가능 |
|  getForEntity(...)   |   GET   | 주어진 URL 주소로 HTTP GET 메서드로 결과는 ResponseEntity로 반환받음 |
|  getForObject(...)   |   GET   | 주어진 URL 주소로 HTTP GET 메서드로 객체로 결과를 반환받음   |
| headForHeaders(...)  | HEATHER | HTTP HEAD요청을 전송<br />지정된 리소스 URL의 HTTP 헤더를 반환(헤더 정보 얻기를 원할때 사용) |
| optionsForAllow(...) | OPTIONS | 주어진 URL 주소에서 지원하는 HTTP 메서드를 조회<br />지정된 리소스 URL의 Allow 헤더를 반환 |
| patchForObject(...)  |  PATCH  | 주어진 URL 주소로 HTTP PATCH 메서드를 실행<br />응답 몸체와 연결되는 결과 객체를 반환 |
|  postForEntity(...)  |  POST   | POST 요청을 보내고 결과로 ResponseEntity를 받음              |
| postForLocation(...) |  POST   | POST 요청을 보내고 결과로 헤더에 저장된 URL을 결과로 반환<br />URL에 데이터를 POST 하며, 새로 생성된 리소스의 URL을 반환 |
|  postForObject(...)  |  POST   | POST 요청을 보내고 객체로 결과를 받음                        |
|       put(...)       |   PUT   | 주어진 URL 주소로 HTTP PUT 메서드를 실행                     |

✔ 해당 메서드들의 오버로딩 형태

1. 가변 인자 리스트에 지정된 URL 매개변수에 URL 문자열(String 타입)을 인자로 받음
2. Map<String, String>에 지정된  URL 매개변수에 URL 문자열을 인자로 받음
3. java.net.URI를 URL 인자로 받으며, 매개변수화 된 URL은 지원X





### RestTemplate의 동작 원리

![image-20220620004544241](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220620004544241.png)

1. 어플리케이션이 RestTemplate를 생성하고, URI, HTTP 메소드 등의 헤더를 담아 요청
2. RestTemplate는 HttpMessageConverter를 사용하여 requestEntity를 요청 메세지로 변환
3. RestTemplate는 ClientHttpRequestFactory로 부터 ClientHttpRequest를 가져와서 요청을 보냄
4. ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신
5. RestTemplate 는 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태움
6. ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리
7. RestTemplate 는 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환
8. 어플리케이션에 반환



### RestTemplate 사용법

#### 		1. 우리가 필요한 시점에 RestTemplate 인스턴스 생성

```java
RestTemplate rest = new RestTemplate();
```

또는 빈으로 선언 후 필요시 주입 가능

```
@Bean
public RestTemplate restTemplate(){
	return new RestTemplate();
}
```



#### 	2-1 리소스 가져오기(GET)

##### <span style="color:blue">1. getForObject</span>

```java
public Ingredient getIngredientById(String ingredientId){
	return rest.getForObject("http://localhost:8080/ingredients/{id}",
								Ingredient.class, IngredientId);
}
```

+ 첫번 째 인자 = URL 문자열

+ 두번 째 인자 = 응답이 바인딩되는 타입
+ 세번 째 ~ = 매개변수 -> 플레이스홀더에 순서대로 대입됨
  + 여기서는 {id}라는 플레이스 홀더에 IngredientId가 대입됨



```java
public Ingredient getIngredientById(String ingredientId){
	Map<String, String> urlVariables = new HashMap<>();
	urlVariables.put("id", ingredientId);
	return rest.getForObject("http://localhost:8080/ingredients/{id}",
                            			Ingredient.class. urlVariables);
}
```

+ Map을 사용해서 URL 변수 지정하기



```java
public Ingredient getIngredientById(String ingredientId){
	Map<String, String> urlVariables = new HashMap<>();
	urlVariables.put("id", ingredientId);
	URI url = UriComponenetsBuilder
					.fromHttpUrl("http://localhost:8080/ingredients/{id}")
					.build(urlVariables);
	return rest.getForObject(url, Ingredient.class);
}
```

##### UriComponentsBuilder

> <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/UriComponentsBuilder.html#fromPath-java.lang.String->

> 여러개의 파라미터들을 연결하여 URL 형태로 만들어주는 기능

사용 예시

```JAVA
@RequestMapping("/login/kakao")
    public void kakao(HttpServletResponse httpServletResponse) throws IOException {

        UriComponents uriComponents = UriComponentsBuilder.newInstance()
            .scheme("https")
            .host("kauth.kakao.com")
            .path("/oauth/authorize")
            .queryParam("client_id", "Insert your Id")
            .queryParam("redirect_uri", "http://localhost:8080/oauth")
            .queryParam("response_type", "code")
            .build(true);
            
        httpServletResponse.sendRedirect(uriComponents.toString());
    }
```



**책에서 사용된 메서드**

+ build(Map<String, ?>)
  + URI에 있는 플레이스 홀더(변수)를 해당 Map의 항목값으로 교체하여 URI를 반환

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220620114653024.png" alt="image-20220620114653024" style="zoom:80%;" align="left"/>

+ fromHttpUrl(String)
  + 주어진 String을 이용해서 URI의 URI 컴포턴트를 반환

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220620114736125.png" alt="image-20220620114736125" style="zoom:100%;" />

✔**URI 와 URI components의 차이?**



##### <span style="color:blue">2. getForEntity</span>

> getForObject()는 응답 결과를 나타내는 도메인 객체를 반환하지만 getForEntity는 ResponseEntity 객체를 반환



```java
public Ingredient getIngredientById(String ingredientId){
	ResponseEntity<Ingredient> ResponseEntity =
		rest.getForEntity("http://localhost:8080/ingredients/{id}",
							Ingredient.class, ingredientId);
    log.info("Fetched time: "+
            responseEntity.getHeaders().getDate());
    return responseEntity.getBody();
}
```

+ 도메인 객체인 식자재 데이터에 추가하여 응답의 Date 헤더 확인



#### 2-2 리소스 쓰기(PUT)

> HTTP PUT 요청 전송
>
> > 직렬화된 후 지정된 URL로 전송되는 Object 타입을 인자로 받음



**특정 식자재 리소스를 새로운 Ingredient 객체의 데이터로 교체**

```java
public void updateIngredient(Ingredient ingredient){
	rest.put("http://localhost:8080/ingredients/{id}",
			 ingredient,
			 ingredient.getId());
}
```

+ 첫번 째 인자 = URL 문자열

+ 두번 째 인자 = 전송될 객체 자체
+ 세번 째 ~ = 매개변수 -> 플레이스홀더에 순서대로 대입됨
  + 여기서는 {id}라는 플레이스 홀더에 IngredientId가 대입됨



#### 2-3 리소스 삭제하기(DELETE)

> HTTP DELETE 요청 전송



```java
public void deleteIngredient(Ingredient ingredient){
	rest.delete("http://localhost:8080/ingredients/{id}",
				ingredient.getId());
}
```

+ 문자열로 지정된 URL과 URL 변수값만 delete()의 인자로 전달



#### 2-4 리소스 데이터 추가하기(POST)

> HTTP POST 요청 전송
>
> > 교재 기준 /Ingredients 엔드포인트에 요청



##### <span style="color:blue">1. postForObject</span>

> post 후 새로 생성된 리소스를 반환받을 때 사용

```java
public Ingredient createIngredient(Ingredient ingredient){
	return rest.postForObject("http://localhost:8080/ingredients",
							  ingredient,
							  Ingredient.class);
}
```

+ 문자열(String 타입) URL과 서버에 전송될 객체 및 이 객체의 타입(리소스 몸체의 데이터와 연관된)을 인자로 받음
  + URL :  "http://localhost:8080/ingredients"
  + 서버에 전송될 객체 : ingredient
  + 이 객체의 타입(리소스 몸체의 데이터와 연관된) : Ingredient.class
  + 네번째 이후 인자는 URL에 플레이스 홀더가 있을 경우 대입을 위한 가변 매개변수 리스트를 전달 가능(Map과 같은)
+ 마지막에 post 요청이 수행된 후 새로 생성된 리소스(여기서는 Ingredent)를 반환받음



##### <span style="color:blue">2. postForLocation</span>

> post후 새로 생성된 리소스의 위치가 필요할 때 사용

```java
public URI createdIngredient(Ingredient ingredient){
	return rest.postForLocation("http://localhost:8080/ingredients",
								ingredient,
								Ingredient.class);
}
```

+ postForObject와 동일하게 작동하지만 리소스 객체 대신 새로 생성된 리소스의 URI를 반환
  + 따라서 해당 객체의 타입(리소스 몸체의 데이터와 연관된)을 인자로 넣을 필요 x



##### <span style="color:blue">3. postForEntity</span>

> 새로 생성된 리소스의 위치와 리소스 객체 모두가 필요할 경우 사용

```java
public Ingredient createdIngredient(Ingredient ingredient){
	ResponseEntity<Ingredient> responseEntity =
			rest.postForLocation("http://localhost:8080/ingredients",
								ingredient,
								Ingredient.class);
								
    log.info("New resource created at " +
    			responseEntity.getHeaders().getLocation());
    return responseEntity.getBody();
}
```



✔**RestTemplate의 단점**

> 우리가 사용하는 API에서 하이퍼링크를 포함해야 한다면 사용 불가



## Traverson

> HATEOAS에 같이 제공, 하이퍼미디어 API를 사용할 수 있는 솔루션
>
> 관계이름으로 원하는 API를 (이동하며) 사용

+ HTTP 메서드를 지원하지 않고 하이퍼링크를 이용하여 다른 URL로 이동
+ 기본적으로 Traverson 인스턴스에 API의 기본 URI와 Accept 헤더를 설정



### 사용 방법

#### 1. 해당 API의 기본 URI를 갖는 객체를 생성

```java
Traverson traverson = new Traverson(URI.create("http://localhost:8080/api"), MediaTypes.HAL_JSON);
```

+ 현재는 타코클라우드의 기본 URL(로컬에서 실행되는)로 지정



### 2. 링크를 따라가면서 API를 사용

```java
ParameterizedTypeReference<Resources<Ingredient>> ingredientType =
	new ParameterizedTypeReference<Resources<Ingredient>>() {};
	
Resources<Ingredient> ingredientRes = 
	traverson
		.follow("ingredients")
		.toObject(ingredientType);
		
Collection<Ingredient> ingredients = ingredientRes.getContent();
```

+ follow(String)
  + 반환값 = Traverson, TraversalBuilder
  + Traverson에 Customizing된 정보로 리소스 간의 단일관계에 대해 설정
  + traverson 인스턴스에서 사용된 URL에 뒤에 /ingredients가 달라붙은것과 동일?
+ toObject(Type)
  + 순회를 시작하고 최종 응답을 지정된 type의 객체로 반환

+ **ParameterizedTypeReference<>**

  > 제너릭을 캡쳐하고 전달할 수 있도록 하는 Type

  + toObject(Type)을 할때 Resources<Ingredient> 형태로 반환해야 하는데 이렇게 리소스 타입을 지정하기 쉽지 않음

  + ParameterizedTypeReference를 생성하여 리소스타입을 지정
    + ParameterizedTypeReference<캡쳐하고 전달하고자 하는 제너릭>

```java
ParameterizedTypeReference<Resources<Taco>> tacoType =
	new ParameterizedTypeReference<Resources<Taco>>() {};
	
Resources<Taco> tacoRes = 
	traverson
		.follow("tacos")
		.follow("recents")
		.toObject(tacoType);
		
Collection<Ingredient> ingredients = ingredientRes.getContent();
```





**✔Traverson VS RestTemplate**

+ Traverson
  + HATEOAS가 활성화 된 API를 이동하면서 해당 API 리소스를 쉽게 가져옴
  + API 리소스를 쓰거나 삭제하는 메서드는 제공 X
+ RestTemplate
  + API에 리소스를 쓰거나 삭제할 수 있음
  + API를 이동하는 것이 어려움



**📌API의 이동과 리소스의 변경이나 삭제를 모두 해야할 경우**

+ RestTemplate과 Traverson 둘다 사용

```java
private Ingredient addIngredient(Ingredient ingredient){
	String ingredientsUrl = traverson
		.follow("ingredients")
		.asLink()
		.getHref();
		
	return rest.postForObject(ingredientsUrl,
							  ingredient,
							  Ingredient.class);
}
```

