## Spring - REST(하이퍼미디어)

### <span style="color:red">1. 하이퍼미디어 사용</span>

#### **HATEOAS(Hypermedia As The Engine Of Application State)**

> API로부터 반환되는 리소스(데이터)에 해당 리소스와 관련된 하이퍼링크들이 포함
>
> 서버가 클라이언트에게 하이퍼 미디어를 통해 정보를 동적으로 제공해주는 것
>
> > 따라서 클라이언트가 최소한의 API URL만 알면 반환되는 리소스와 관련하여 처리 가능한 다른 API URL들을 알아내어 사용가능

+ 지금까지 생성했던 기본적인 API
  + 해당 API를 사용하는 클라이언트가 API의 URL 스킴(scheme)을 알아야 함
    + 클라이언트에서는 /design/recent의 GET 요청을 하드코딩하여 최근 생성된 타코 리스트를 얻을 수 있음
  + API 클라이언트 코드에서는 흔히 하드코딩된 URL 패턴을 사용하고 문자열로 처리
  + API의 URL 스킴이 변경되면 정상동작을 수행하기 힘듬
    + 기존의 REST API의 경우 API의 엔드포인트 URL이 정해지면 이를 변경하기 어렵다는 단점이 존재
    + API의 기존 URL을 변경하게 되면 이를 사용하는 모든 클라이언트가 함께 수정되어야 하기 때문



#### HAL(Hypertext Application Language)

> JSON 응답에 하이퍼링크를 포함시킬 때 주로 사용되는 형식

+ _links 속성
  + 클라이언트가 관련 API를 수행할 수 있는 하이퍼링크를 포함
  + 클라이언트 애플리케이션이 타코 리스트의 특정 타코에 대해 HTTP 요청을 수행해야 할 때 해당 타코 리소스의 URL을 지정하지 않아도 됨

+ 의존성 추가

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```



#### 1. 하이퍼링크 추가하기

> HATEOAS는 하이퍼링크 리소스를 나타내는 2개의 기본 타입인 Resource와 Resources를 제공

+ Resource 타입 : 단일 리소스
+ Resources 타입 : 리소스 컬렉션
+ 두 타입 모두 다른 리소스를 링크할 수 있음
+ 이때, 전달하는 링크는 스프링 MVC 컨트롤러 메서드에서 반환될 때 클라이언트가 받는 JSON(또는 XML)에 포함됨



```java
//RESTful API
@GetMapping("/recent")
  public Iterable<Taco> recentTacos() {   
    PageRequest page = PageRequest.of(
            0, 12, Sort.by("createdAt").descending());
    return tacoRepo.findAll(page).getContent();
  }

//하이퍼링크 추가
@GetMapping("/recent")
public Resources<Resource<Taco>> recentTacos(){
	PageRequest page = PageRequest.of(
			0, 12, Sort.by("CreatedAt").descending());
	)
	
	List<Taco> tacos = tacoRepo.findAll(page).getContent();
	Resources<Resource<Taco>> recentResources = Resources.wrap(tacos);
	
	recentResources.add(
			new Link("http://localhost:8080/design/recent", "recents"));
    return recentResources;
}

//HATEOAS 링크 빌더 중 하나인 ControllerLinkBuilder 사용
Resources<Resource<Taco>> recentResources = Resources.wrap(tacos);
recentResources.add(
	ControllerLinkBuilder.linkTo(DesignTacoController.class)
						 .slash("recent")
    					 .withRel("recents")
);

//methodOn() 사용
Resources<Resource<Taco>> recentResources = Resources.wrap(tacos);
recentResources.add(
	linkTo(methodOn(DesignTacoController.class).recentTacos())
    .withRel("recents")
);
```

+ Resources.wrap()
  + recentTacos의 반환타입인 Resources<Resource<Taco>>의 인스턴스로 타코 리스트를 매핑
+ recentResources.add() 를 통해 링크 추가
  + 타코 전체 리스트에 대한 링크가 추가된 것이지 타코 리소스 자체나 각 타코의 식자재에 대한 링크는 추가되지 않음
  + localhost:8080의 경우 개발용에서만 사용가능하기에 위와같은 하드코딩이 좋은 방식은 아님

![image-20220613211957330](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220613211957330.png)

![image-20220613212048708](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220613212048708.png)

```json
{
  "_links" : {
    "recents" : {
      "href" : "http://localhost:8080/design/recent"
    }
  }
}
```

+ ControllerLinkBuilder
  + 호스트 이름을 하드코딩 x, /design 경로 x
  + 기본 경로가 /design인 링크를 DesignTacoController에 요청
  + ControllerLinkBuilder는 이 컨트롤러의 기본 경로를 사용해서 Link객체를 생성
  + .slash()
    + 슬래시와 인자로 전달된 값을 URL에 추가
  + .withRel()
    + 해당 Link의 관계 이름(relation name - 링크 참조 시 사용)을 지정

+ methodOn()
  + 컨트롤러 클래스인 DesignTacoController를 인자로 받아 recentTacos() 메서드를 호출할 수 있게 도와줌
  + 해당 컨트롤러의 기본 경로와 recentTacos()의 매핑 경로 모두를 결정하는데 사용



#### 2. 리소스 어셈블러 생성하기

**현재 목표** : 리스트에 포함된 각 타코 리소스에 대한 링크 추가



**해결책 1.** 반복 루프에서 Resources 객체가 가지는 각 Resource<Taco> 요소에 Link를 추가하는 것

+ 리스트를 반환하는 API 코드 마다 르프를 실행 -> 번거로움



**해결책 2.** Taco객체를 새로운 TacoResource 객체로 변환하는 유틸리티 클래스 정의(Taco 클래스와 유사하지만 링크를 추가로 가질 수 있음)

```java
public class TacoResource extends ResourceSupport {
  
  @Getter
  private final String name;

  @Getter
  private final Date createdAt;

  @Getter
  private final List<IngredientResource> ingredients;
  
  public TacoResource(Taco taco) {
    this.name = taco.getName();
    this.createdAt = taco.getCreatedAt();
    this.ingredients = taco.getIngredients();
  }
}

public class TacoResourceAssembler
       extends ResourceAssemblerSupport<Taco, TacoResource> {

  public TacoResourceAssembler() {
    super(DesignTacoController.class, TacoResource.class);
  }
  
  @Override
  protected TacoResource instantiateResource(Taco taco) {
    return new TacoResource(taco);
  }

  @Override
  public TacoResource toResource(Taco taco) {
    return createResourceWithId(taco.getId(), taco);
  }
}
```

**{TacoResource}**

+ Taco 클래스와 **유사점**
  + name, createdAt, ingredients 속성을 가짐
+ Taco 클래스와 차이점
  + TacoResource는 ResourceSupport의 서브클래스로서 Link 객체 리스트와 이것을 관리하는 메서드를 상속받음
  + Taco클래스의 id 속성을 가지지 않음
    + 데이터베이스에서 필요한 ID를 API에 노출시킬 필요가 없기 때문
+ TacoResource(Taco taco)
  + 생성자
  + Taco 객체를 인자로 받아 해당 객체의 속성값을 자신의 속성에 복사

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220614001834877.png" alt="image-20220614001834877" style="zoom:67%;" align = "left"/>

**{TacoResourceAssembler}**

+ TacoResourceAssembler()
  + 생성자
  + 슈퍼 클래스인 ResourceAssemblerSupport의 기본생성자 호출
  + 이때 TacoResource를 생성하면서 만들어지는 링크에 포함되는 URL의 기본 경로를 결정하기 위해 DesignTacoController를 사용

<img src="C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220614003104167.png" alt="image-20220614003104167" style="zoom:80%;" align="left"/>

+ instantiateResource()
  + 인자로 전달된 Taco 객체로 TacoResource 인스턴스를 생성하도록 오버라이드
  + Resource 인스턴스만 생성

+ toResource()
  + Resource 인스턴스를 생성하면서 Taco 객체의 id속성값으로 생성되는 self 링크가 URL에 자동으로 지정
  + instantiateResource() 에 링크도 추가한 케이스

![image-20220614003540739](C:\Users\LG\AppData\Roaming\Typora\typora-user-images\image-20220614003540739.png)



**{TacoResourceAssembler를 사용해서 DesignTacoController 수정하기}**

```java
//기존의 코드
@GetMapping("/recent")
  public Iterable<Taco> recentTacos() {                 
    PageRequest page = PageRequest.of(
            0, 12, Sort.by("createdAt").descending());
    return tacoRepo.findAll(page).getContent();
  }

//TacoResourceAssembler 사용
@GetMapping("/recent")
  public Resources<TacoResource> recentTacosH() {
    PageRequest page = PageRequest.of(
            0, 12, Sort.by("createdAt").descending());
    List<Taco> tacos = tacoRepo.findAll(page).getContent();
	
    //리퍼지토리로부터 타코들을 가져와 Taco객체 리스트(tacos)에 저장
    //Taco 객체로 Resource 인스턴스를 생성하면서 링크도 추가 -> tacos가 List였기 때문에 Resource 객체를 저장한 List를 생성
    List<TacoResource> tacoResources = new TacoResourceAssembler().toResources(tacos);
    Resources<TacoResource> recentResources = new Resources<TacoResource>(tacoResources);
    
    recentResources.add(
        linkTo(methodOn(DesignTacoController.class).recentTacos())
        .withRel("recents"));
    return recentResources;
 }
```

-------

**현재까지 진행 상황** : 타코들이 포함된 리스트 자체의 recents 링크를 가지는 타코 리스트 생성

**추가 목표** : 각 타코의 식자재(Ingredient 객체)에는 여전히 링크가 없음

**해결 방안** : 타코에 링크를 넣은 방법 그대로 동일하게 적용(어셈블러)



```java
//Assembler
class IngredientResourceAssembler extends 
          ResourceAssemblerSupport<Ingredient, IngredientResource> {

  public IngredientResourceAssembler() {
    super(IngredientController.class, IngredientResource.class);
  }

  @Override
  public IngredientResource toResource(Ingredient ingredient) {
    return createResourceWithId(ingredient.getId(), ingredient);
  }
  
  @Override
  protected IngredientResource instantiateResource(
                                            Ingredient ingredient) {
    return new IngredientResource(ingredient);
  }
}

//Resource
public class IngredientResource extends ResourceSupport {

  @Getter
  private String name;

  @Getter
  private Type type;
  
  public IngredientResource(Ingredient ingredient) {
    this.name = ingredient.getName();
    this.type = ingredient.getType();
  }
}

//TacoResource에 Ingredient 객체 대신 IngredientResource 객체를 처리
public class TacoResource extends ResourceSupport {

  //IngredientResourceAssembler의 static final 인스턴스 생성
  private static final IngredientResourceAssembler ingredientAssembler = new IngredientResourceAssembler();
  
  @Getter
  private final String name;

  @Getter
  private final Date createdAt;

  @Getter
  private final List<IngredientResource> ingredients;
  
  public TacoResource(Taco taco) {
    this.name = taco.getName();
    this.createdAt = taco.getCreatedAt();
    //IngredientResourceAssembler의 toResources()를 이용해서 Taco 객체의 Ingredient 리스트를 IngredientResources로 변환
    this.ingredients = 
        ingredientAssembler.toResources(taco.getIngredients()); //this.ingredients = taco.getIngredients();
  }
}
```



#### 3. embedded 관계 이름 짓기

```json
{
	"_embedded":{
		"tacoResourceList": [
			...
		]
	}
}
```

+ "tacoResourceList"
  + Resources 객체가 List<TacoResource>로 부터 생성되었다는 것을 나타냄
+ 만약 자바로 정의된 리소스 타입 클래스의 이름과 JSON 필드 이름 간의 결합도를 낮추고 싶을 경우
  + @Relation 어노테이션 사용

```java
//TacoResource의 List<TacoResource> 이름 대신 tacos라는 Json에서의 필드이름 지정 가능
@Relation(value="taco", collectionRelation="tacos")
public class TacoResource extends ResourceSupport {
	...
}

//JSON
{
	"_embedded":{
		"tacos": [
			...
		]
	}
}
```



### <span style = "color:red">3. 데이터 기반 서비스 활성화</span>

**스프링 데이터 REST**

> 스프링 데이터의 또 다른 모듈
>
> 스프링 데이터가 생성하는 리퍼지터리의 REST API를 자동 생성

+ 스프링 데이터 REST를 우리 빌드에 추가하면 우리가 정의한 각 레퍼지터리 인터페이스를 사용하는 API를 얻을 수 있음



**의존성**

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```



**장점**

+ 스프링 데이터를 사용중인 프로젝트에서 REST API를 노출시킬 수 있음
+ 스프링 데이터가 생성한 모든 레퍼지토리(스프링 데이터 JPA, 스프링 데이터 몽고)의 REST API가 자동 생성 될수 있도록 스프링 데이터 REST가 자동-구성
  + /ingredients의 POST 요청을 하여 새로운 식자재를 생성도 가능
  + /ingredients/FLTO의 DELETE 요청을 하여 'Flour Tortilla' 식자재를 삭제도 가능
+ 스프링 데이터 REST가 자동 생성한 API의 기본 경로(spring.data.rest.base-path 속성에 설정가능)

```properties
spring:
	data:
		rest:
			base-path: /api
```

**예시**

+ 위의 경우 기본경로를 /api로 설정하였으므로 식자재의 엔드포인트는 /api/ingredients



#### 1. 리소스 경로와 관계 이름 조정

**스프링 데이터 REST**

> 엔드 포인트가 노출
>
> > 스프링 데이터 레포지토리의 엔드포인트를 생성할 때 해당 엔드포인트와 관련된 엔티티 클래스 이름을 복수형을 사용

+ Ingredient 클래스 -> /ingredients

+ Taco 클래스 -> /api/tacos 가 아닌 /api/tacoes 로 지정

```json
% curl localhost:8080/api/tacoes
```

+ @RestResource(rel="tacos", path="tacos")
  + rel : 관계 이름
  + path : 경로
  + 즉 관계 이름과 경로를 tacos로 바꿈



**{Taco}**

```java
@Data
@Entity
@RestResource(rel="tacos", path="tacos")
public class Taco {

  @Id
  @GeneratedValue(strategy=GenerationType.AUTO)
  private Long id;
  
  @NotNull
  @Size(min=5, message="Name must be at least 5 characters long")
  private String name;
  
  private Date createdAt;

  @ManyToMany(targetEntity=Ingredient.class)
  @Size(min=1, message="You must choose at least 1 ingredient")
  private List<Ingredient> ingredients;

  @PrePersist
  void createdAt() {
    this.createdAt = new Date();
  }
}
```



#### 2. 페이징과 정렬

**홈 리소스의 링크**

> 선택적 매개변수인 page, size, sort를 제공

+ /api/tacos와 같은 컬렉션(여러 항목이 포함된) 리소스를 요청하면 기본적으로 한 페이지 당 20개의 항목이 반환
+ page와 size 매개변수를 지정하면 요청에 포함될 페이지 번호와 페이지 크기 조정 가능

```json
//크기가 5인 페이지 요청, GET 요청
$ curl "localhost:8080/api/tacos?size=5"

//5개 이상의 타코가 있다고 가정할 때, 2번째 페이지의 타코 요청(page 매개변수는 0부터 시작)
$ curl "localhost:8080/api/tacos?size=5&page=1"
```

+ curl과 같은 여러 명령행 셸에서는 요청 속 앰퍼샌드(&)를 포함하기에 URL 전체를 겹따옴표(" ")로 둘러싸야함



**HATEOAS의 페이지 링크 요청**

```json
"_links" : {
	"first" : {
		"href" : "http://localhost:8080/api/tacos?page=0&size=5"
	},
	"self" : {
		"href" : "http://localhost:8080/api/tacos"
	},
	"next" : {
		"href" : "http://localhost:8080/api/tacos?page=1&size=5"
	},
	"last" : {
		"href" : "http://localhost:8080/api/tacos?page=2&size=5"
	},
	"profile" : {
		"href" : "http://localhost:8080/api/profile/tacos"
	},
	"recents" : {
		"href" : "http://localhost:8080/api/tacos/recent"
	},
}
```

+ HATEOAS는 처음(first), 마지막(last), 다음(next), 이전(previous)페이지의 링크를 요청응답에 제공
+ API의 클라이언트는 현재 페이지가 어딘지 계속 파악하면서 매개변수와 URL을 연관시킬 필요가 없음
  + 링크 이름으로 이런 페이지를 이동하는 링크들 중 하나를 찾으면 됨



**SORT 매개변수**

> 엔티티 속성을 기준으로 결과 리스트를 정렬 가능

```
$ curl "localhost:8080/api/tacos?sort=createdAt,desc&page=0&size=12"
```

+ createdAt 속성을 기준으로 내림차순한 첫번째(page=0) 페이지의 12개 항목을 가져옴



#### 3. 커스텀 엔드포인트 추가

**스프링 데이터 REST**

> 스프링 데이터 레퍼지터리의 CRUD 작업을 수행하는 엔드포인트 생성을 잘 하도록 함

+ 기본적은 CRUD API로부터 탈피하여 우리 나름의 엔드포인트를 생성해야할 경우는?
  + @RestController 애노테이션이 저장된 빈을 구현해서 엔드포인트 보충
    + **고려할 점**
      + 우리의 엔드포인트 컨트롤러는 스프링 데이터 REST의 **기본경로로 매핑되지 않음**
        + 스프링 데이터 REST의 기본경로를 포함하여 우리가 원하는 기본 경로가 앞에 붙도록 매핑
        + 기본 경로가 변경될 시 해당 컨트롤러의 매핑이 일치되도록 수정 필요
      + 우리의 엔드포인트는 스프링 데이터 REST 엔드포인트에서 반환되는 리소스의 **하이퍼링크에 자동으로 포함 X**
        + 클라이언트가 관계이름을 사용해서 커스텀 엔드포인트를 찾을 수 있음



##### 2. 기본경로로 매핑문제 해결

> @RepositoryRestController가 지정된 컨트롤러의 모든 경로 매핑은 spring.data.rest.base-path 속성의 값(/api로 구성했던)이 앞에 붙은 경로

+ DesignTacoController는 우리가 필요없는 여러 핸들러 메서드를 갖고있으므로 새로운 컨트롤러 작성



**{RecentTacosController}**

```java
@RepositoryRestController
public class RecentTacosController {

  private TacoRepository tacoRepo;

  public RecentTacosController(TacoRepository tacoRepo) {
    this.tacoRepo = tacoRepo;
  }

  @GetMapping(path="/tacos/recent", produces="application/hal+json")
  public ResponseEntity<Resources<TacoResource>> recentTacos() {
    PageRequest page = PageRequest.of(
                          0, 12, Sort.by("createdAt").descending());
    List<Taco> tacos = tacoRepo.findAll(page).getContent();

    List<TacoResource> tacoResources = 
        new TacoResourceAssembler().toResources(tacos);
    Resources<TacoResource> recentResources = 
            new Resources<TacoResource>(tacoResources);
    
    recentResources.add(
        linkTo(methodOn(RecentTacosController.class).recentTacos())
            .withRel("recents"));
    return new ResponseEntity<>(recentResources, HttpStatus.OK);
  }
}
```

+ @RepositoryRestController
  + 스프링 데이터 REST의 기본경로를 요청 매핑에 적용하기 위해 지정
+ @GetMapping()
  + path="/tacos/recent"
    + @RepositoryRestController의 영향으로 경로는 "api/tacos/recent"의 GET 요청을 처리
+ @RepositoryRestController와 @RestController
  + @RepositoryRestController
    + 핸들러 메서드의 반환값을 요청 응답의 몸체에 자동으로 수록X
    + 해당 메서드에 @ResponseBody 애노테이션을 지정하거나 해당 메서드에서 응답 데이터를 포함하는 ResponseEntity를 반환



**현재까지의 진행 상황** : /api/tacos/recent의 Get 요청을 할 때 가장 최근에 생성된 타코를 12개 까지 반환

**추가 해결 문제 **: /api/tacos를 요청할 때 여전히 하이퍼링크 리스트에 나타나지 않음

----------------

##### 2. 커스텀 하이퍼링크를 스프링 데이터 엔드포인트에 추가

> 리소스 프로세서 빈을 선언하면 스프링 데이터 REST가 자동으로 포함시키는 링크 리스트에 해당 링크를 추가
>
> > ResourceProcessor를 제공 - API를 통해 리소스가 반환되기 전에 리소스를 조작하는 인터페이스

```java
@Configuration
public class SpringDataRestConfiguration {

  @Bean
  public ResourceProcessor<PagedResources<Resource<Taco>>> tacoProcessor(EntityLinks links) { //tacoProcessor 메서드
    
    return new ResourceProcessor<PagedResources<Resource<Taco>>>() { //익명 내부 클래스로 ResourceProcessor 정의
      @Override
      public PagedResources<Resource<Taco>> process(PagedResources<Resource<Taco>> resource){ //process 메서드
        resource.add(
            links.linkFor(Taco.class)
                 .slash("recent")
                 .withRel("recents"));
        return resource;
      } //process 메서드 끝
    }; //ResourceProcessor<PagedResources<Resource<Taco>>>()
  } //tacoProcessor 메서드 끝
}
```

+ ResourceProcessor<Resource <Taco>> 타입(/api/tacos 엔드포인트의 반환 타입)의 리소스에 recent 링크를 추가
+ PagedResources<Resource<Taco>>가 반환되면 가장 최근에 생생된 타코들의 링크를 받게 되며, /api/tacos 요청 응답에도 해당 링크들이 포함





----------

*Refer*ence

https://docs.spring.io/spring-hateoas/docs/0.25.0.RELEASE/api/org/springframework/hateoas/Resources.html#wrap-java.lang.Iterable-