---
authors:
- younjin
categories:
- News
- Pivotal 
- Spring Framework
date: 2017-11-27T06:34:12-04:00
short: |
  2017년 11월 기준 스프링5 프레임워크에 추가된 새로운 기능에 대한 간단한 정리 by 이창재(Jay Lee)
title: 스프링 프레임워크 5의 새로운 기능
--- 

스프링 프레임워크의 최신 메이저 버전인 5가 지난 9월에 발표된 이후 중요한 업데이트에 대해 간단하게 정리해 본다. 정식 문서는 [여기](https://docs.spring.io/spring/docs/current/spring-framework-reference/)에서 참고해 볼 수 있겠다. 본 내용은 한국 스프링 사용자 그룹에서 진행한 Java & Spring 행사에서 소개 된 내용으로, 대부분 피보탈의 이창재 (Jay Lee, jaylee@pivotal.io)님께서 작성해 주신것을 기반으로 블로그화 했다.  


Juregen Hoeller 가 작성한 업데이트 리뷰 정보는 [여기](https://github.com/spring-projects/spring-framework/wiki/What's-New-in-Spring-Framework-5.x)


---

### Spring 5 Major Changes 

* Java 9 Compatible 
* JaveEE 8 Support
* HTTP/2 Support
* Reactive: WebFlux, Router Functions
* Functional Bean Configuration 
* JUnit 5 
* Portlet Deprecated 
 
---

#### Baseline Changes - versions 

* JDK 8+ 
* Servlet 3.1+
* JMS 2.0
* JPA 2.1
* JavaEE 7+

---

#### Java 9 Jigsaw

**SPR-13501: Declare Spring modules with JDK 9 module metadata is still Open**

- Manifest-Version: 1.0
- Implementation-Title: spring-core
- **Automatic-Muodule-Name: spring.core** 
- Implemetation-Version: 5.0.2.BUILD-SNAPSHOT 
- Created-By: 1.8.0_144 (Oracle Corporation)

---

#### JavaEE 8 GA - Sep 21, 2017

- CDI 2.0 (JSR 365)
- **JSON-B 1.0 (JSR 367)**
- **Servlet 4.0 (JSR 369)**
- JAX-RS 2.1 (JSR 370)
- JSF 2.3 (JSR 372)
- JSON-P 1.1 (JSR 374)
- Security 1.0 (JSR 375)
- **Bean Validation 2.0 (JSR 380)**
- JPA 2.2 : Maintenance Release 

---

#### Java API for JSON Binding (JSON-B) 1.0 - JSR 367

**Standard Binding Layer for converting Java Objects to/from JSON**

- Thread Safe
- Apache Johnzon 
- Eclipse Yasson 

~~~xml
<!-- required dependency for Johnzon --> 
<dependency>
    <groupId>org.apache.geronimo.specs</groupId> 
   <artifactId>geronimo-json_1.1_spec</artifactId>
    <version>1.0</version> 
</dependency>
 <dependency>
    <groupId>org.apache.johnzon</groupId> 
   <artifactId>johnzon-jsonb</artifactId>
   <version>1.1.5</version>
 </dependency>

<!-- required dependency for Yasson -->

<dependency>
    <groupId>org.glassfish</groupId>
    <artifactId>javax.json</artifactId>
    <version>1.1.2</version>
 </dependency>
 <dependency>
    <groupId>org.eclipse</groupId>
    <artifactId>yasson</artifactId> 
   <version>1.0.1</version>
 </dependency>
~~~

~~~java
Person person = new Person(); 
person.name = "Jay Lee"; 
person.age = 1; 
person.email = “jaylee@pivotal.io”; 

 Jsonb jsonb = JsonbBuilder.create();
 String JsonToString = jsonb.toJson(person);

  System.out.println(JsonToString );

  person = jsonb.fromJson("{\"name\":\”Jay Lee\",\"age\":1,\”email\":\”jaylee@pivotal.io\"}", Person.class);
~~~

--- 

#### Java API for JSON Binding (JSONB-B) 1.0 - JSR 367

**New HttpMessageConverter for JSON-B in Spring5**

~~~java
@Bean
public HttpMessageConverters customConverters() {
    Collection<HttpMessageConverter<?>>messageConverters = new ArrayList<>();
    JsonbHttpMessageConverter jsonbHttpMessageConverter = new JsonbHttpMessageConverter();
    messageConverters.add(jsonbHttpMessageConverter);
    
    returen new HttpMessageConverters(true, messageConverters); 
}
~~~

#### Servlet 4.0 - HttpServletMapping 

**Provide Runtime Discovery of URL Mappings**

~~~java
@GetMapping("/servlet4") 
public void index(final HttpServletRequest request) { 
    HttpServletMapping mapping = request.getHttpServletMapping();
     System.out.println(mapping.getServletName()); 
    System.out.println(mapping.getPattern());
     System.out.println(mapping.getMappingMatch().name());
     System.out.println(mapping.getMatchValue());
 }
~~~ 

#### Bean Validation 2.0 - JSR 380 
[SPR-13482](https://jira.spring.io/browse/SPR-13482)

**1.1 was introduced in 2013, lack of supports for new Java8, 9** 

- New JDK Types including LocalTime, Optional and etc. 
- Lambda 
- Type Annotation 
- @Email, @Positive, @PositiveOrZero, @Negative, @NegativeOrZero, @PastOrPresent, @FutureOrPresent, @NotEmpty, and @NotBlank

---

### Spring 5 Major Changes 

- Logging Enhancement
- Performace Enhancement 
- Functional Bean Configuration 
- JSR 305
- Spring MVC : HTTP/2 Support
- Reactive: WebFlux, Router Functions 
- JUnit 5

---

#### Spring 5 - XML Configuration Changes 

**Streamlined to use unversioned Schema** 
[SPR-13499](https://jira.spring.io/browse/SPR-13499)

- Always resolved against latest **xsd** files; no support for deprecated features. 
- Version-specific declarations still supported but validated against latest schema. 

--- 

#### Spring 5 - Loggin Enhancement 

**spring-jcl replaces Commons Logging by default** 

- Autodetecting Log4j 2.x, SLF4J, and JUL (java.util.logging) by Class Loading 

--- 

#### Spring 5 - Component Scanning Enhancement 

**Component scanning time reduced by index, imporving Start Up Time**

- META-INF/spring.components is created Compile Time 
- @Indexed Annotation 
- org.springframework.context.index.CandidateComponentsIndex 
- -Dspring.index.ignore=true to fall back to old mechanism 

~~~xml
<!-- Dependency needed --> 
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-indexer</artifactId>
    <optional>true</optional>
 </dependency>
~~~

~~~java

// without @Indexed - old mechanism  
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented 
public @interface Component {
    String value() default "";
}

// with @Indexed 
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed  
public @interface Component {
    String value() default "";
}

~~~

#### Spring 5 - Functional Bean Configuration 

**Support for Functional Bean Registration in GenericApplicationContext, AnnotationConfigApplicationContext**
[SPR-14832](https://jira.spring.io/browse/SPR-14832)

- Very effcient, no reflection, no CGLIB proxies involved 
- Lambda with Supplier act as a FactoryBean 

~~~java
@Autowired
GenericApplicationContext ctx;

@Test
public void functionalBeanTest() {
    ctx.registerBean(Person.class, () -> new Person());
    ctx.registerBean("personService", Person.class,
        () -> new Person(), bd -> bd.setAutowireCandidate(false)); 
    ctx.registerBean("carService", Car.class,
        () -> new Car(), bd -> bd.setScope(BeanDefinition.SCOPE_PROTOTYPE));
}

~~~

#### Spring 5 - JSR 305 

**SPR-15540 Introduce null-safety of Spring Framework API** 
[SPR-15540](https://jira.spring.io/browse/SPR-15540)

- Becomes Kotlin Friendly (-Xjsr305=strict as of 1.1.51)

**New Annotation org.springframework.lang.NonNull, org.springframework.lang.Nullable leverages JSR 305** 

~~~java
@Target({ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Nonnull
@TypeQualifierNickname
public @interface NonNull {
    
}
~~~

--- 

### Spring 5 - Spring MVC 

**HTTP/2 and Servlet 4.0** 

- PushBuilder
- WebFlux
- Support Reactor 3.1, RxJava 1.3, 2.1 as return values 
- JSON Binding API 
- Jackson 2.9 
- Protobuf 3.0 
- URL Matching 

--- 

#### Spring 5 - HTTP/2 Support Containers 

- Tomcat 9.0 
- Jetty 9.3 
- Undertow 1.4

ex. Spring Boot 
~~~xml
<dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-juli</artifactId>
    <version>9.0.1</version>
</dependency>
~~~

#### Spring 5 MVC - Multipart File Size 

**MaxUploadSizeExceededException will be thrown for multipart size overun** 

- Default Value 
- Two Properties (Default Value)
- spring.servlet.multipart.max-file-size=1MB 
- spring.servlet.multipart.max-request-size=10MB 

--- 

#### Spring 5 MVC - URL Matcher Changed 

**PathPatternParser is alternative to Traditional AntPathMatcher for URL Matching**

- org.springframework.web.util.pattern.PathPattern 

~~~text
Examples:
/pages/t?st.html —/pages/test.html, /pages/tast.html  /pages/toast.html
/resources/*.png — matches all .png files in the resources directory
/resources/** — matches all files underneath the /resources/ path, including /resources/image.png and /resources/css/spring.css
/resources/{*path} — matches all files underneath the /resources/ path and captures their relative path in a variable named "path"; /resources/image.png will match with ”path" -> "/image.png", and /resources/css/spring.css will match with ”path" -> "/css/spring.css"
/resources/{filename:\\w+}.dat will match /resources/spring.dat and assign the value "spring" to the filename variable
~~~

#### Spring 5 MVC - Throwing Exception from Controller 

**ResponseStatusException is Introduced to throw Exception in MVC** 

- SPR-14895: Allow HTTP status exceptions to be easily thrown from Controllers 
- ResponseEntity(HttpStatus.BAD_REQUEST)? 

~~~java
@GetMapping("/throw")
public void getException() {
    throw new ResponseStatusException(HttpStatus, BAD_REQUEST, "request invalid"); 
}
~~~

#### Spring 5 WebFlux -WebClient 

**Reactive Web Client introduced in Spring 5, alternative to RestTemplate** 

- AsyncRestTemplate is deprecated


~~~java
WebClient client=WebClient.create();

Mono<Person> person=client
    .get()
    .uri("http://jay-person.cfapps.io/{name}".name)
    .retrieve()
    .bodyToMono(Person.class);
~~~  


#### Spring 5 WebFlux - WebTestClient 

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureWebTestClient
public class MicrometerApplicationsTests {
    @Autowired
    WebTestClient client;
    
    @Test  
    public void testWebClient() {
        client.get().uri("/person/Jay")
        .exchange()
        .expectBody(Person.class)
        .consumeWith(result -> assertThat(result.getResponseBody().getName()).isEqualTo("Jay Lee"));
    }
}
~~~


#### Spring 5 WebFlux - Server

**RouterFunction is introduced for functional programming** 

~~~java
RouterFunction<?> route = route(GET("/usrs"), request -> 
    ok().body(repository.findAll(), Person.class)
    .andRoute(GET("/usrs/{id}", request -> 
        ok().body)repository.findOne(request.pathVariable("id")), Person.class)
    .andRoute(POST("/users"), request ->
        ok().build(repository.save(request.bodyToMono(Person.class).then())
);
~~~

#### WebFlux server (functional)

~~~java
RouterFunction<?> route = route(GET("/users"), request ->
	ok().body(repository.findAll(), User.class))
.andRoute(GET("/users/{id}"), request ->
	ok().body(repository.findOne(request.pathVariable("id")), User.class))
.andRoute(POST("/users"), request ->
	ok().build(repository.save(request.bodyToMono(User.class)).then())
);
~~~

### Spring Boot and Cloud 

**Spring Boot 2.0 will be GA in Feb 2018** 

- Spring Cloud Finchley Release Train based on Spring Boot 2.0 
- Spring Cloud Gateway 
- Spring Cloud Function 


--- 

## Spring 5 - Deprecated APIs 

**HTTP/2 and Servlet 4.0** 

- Hibernate 3, 4
- Portlet 
- Velocity 
- JasperReports 
- XMLBeans 
- JDO 
- Guava 



--- 

# See ALso 

- [피보탈 엔지니어링 블로그](http://engineering.pivotal.io)
- [넷플릭스 혼돈의 제왕](https://www.youtube.com/watch?v=OczG5FQIcXw&t=5s)
- [Cloud Native Java by Josh Long](https://www.youtube.com/watch?v=ZdpZlqumymM&t=1s)
- [로컬 플레이 그라운드: PCFDEV](https://pcfdev.io)
- [60일 무료 클라우드 파운드리](https://run.pivotal.io)
