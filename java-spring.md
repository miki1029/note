## Springframework
[Spring Framework Reference Documentation](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/)  
[ㄴRest Message Conversion](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#rest-message-conversion)  
[Spring 4.x Web Application 살펴보기](http://www.slideshare.net/ihoneymon/spring-4x-web-application)  
[Tomcat & Spring Bootstrapping Sequence](http://brantiffy.axisj.com/archives/232)  

### 스프링 버젼별 변화
스프링 3.2

* 스프링 3.2에서의 가장 큰 변화는 스프링 MVC의 기능 향상이다.
* <http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/new-in-3.2.html>

스프링 MVC 변화

* web.xml 자바 설정으로 대체
  * AbstractDispatcherServletInitializer 또는 서브 클래스 AbstractAnnotationConfigDispatcherServletInitializer를 통해 DispatcherServlet을 설정할 수 있다.
* 테스트 지원
  * 스프링 MVC 테스트 프레임워크 포함
  * RestTemplate 기반의 클라이언트 테스트 지원
* 서블릿 3 지원
  * 비동기식 요청 : 요청 처리를 분리시키고, 분리된 스레드 내에서 처리되도록 한다.
* 애너테이션 추가
  * @ControllerAdvice : 공통의 @ExceptionHandler, @InitBinder, @ModelAttribute 메소드가 하나의 클래스에 집합되도록, 모든 컨트롤러에 적용되도록 만들어 준다.
  * @MatrixVariable : 요청의 행렬 변수를 핸들러 메소드 파라미터로 바인딩
* 전체 콘텐츠 협상
  * 기존 ContentNegotiatingViewResolver를 통해서만 가능했지만 스프링 MVC 컨트롤러 메소드에서도 가능하다.
* ResponseEntityExceptionHandler 추가
  * DefaultHandlerExceptionResolver의 대안
  * ModelAndView 대신 ResponseEntity\<Object\>를 리턴함
* RestTemplate과 @RequestBody 인자의 Generic type 지원
* RestTemplate과 @RequestMapping 메소드의 HTTP PATCH 메소드 지원
* 인터셉터 URL 패턴 지원
* Tiles 3 지원

다른 개선점

* @Autowired, @Value, @Bean을 새로운 애너테이션 선언을 위한 메타 애너테이션으로 사용할 수 있다.
* @DateTimeFormat 애너테이션 변경
  * 더 이상 JodaTime에 강력한 의존성을 갖지 않음
  * JodaTime이 존재하면 사용되고 없으면 SimpleDateFormat이 사용됨
* 선언적 캐싱 지원은 JCache 0.5를 위한 초기 지원을 가진다.
* 날짜와 시간을 파싱하고 렌더링하는 글로벌 포맷을 정의한다.
* 통합 테스트는 WebApplicationContext를 설정하고 로드한다.
* 통합 테스트는 요청 범위와 세션 범위의 빈에 대하여 테스트한다.


스프링 4

<http://docs.spring.io/spring/docs/current/spring-framework-reference/html/new-in-4.0.html>

* Deprecated 패키지 및 모듈 삭제
  * API Difference Report : http://docs.spring.io/spring-framework/docs/3.2.4.RELEASE_to_4.0.0.RELEASE/
* 웹 소켓 프로그래밍 지원
  * JSR-356 자바 API 지원
  * 매우 높은 수준의 메시지 기반 모델인 SockJS, STOMP 서브 프로토콜 지원
* 자바 8 지원
  * 람다식을 통해 읽기에 훨씬 깨끗하고 쉬운 콜백 인터페이스(JdbcTemplate의 RowMapper와 같은)로 작업 가능
  * 자바 8에서 새로 추가된 JSR-310의 데이터 및 시간 API 사용
  * <http://www.oracle.com/technetwork/articles/java/jf14-date-time-2125367.html>
* Groovy 지원
  * Groovy DSL을 사용하여 외부 bean 설정이 가능
  * <http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#groovy-bean-definition-dsl>
* 조건부 빈 생성 : @Conditional을 통해 개발자가 정의한 조건이 만족되었을 때만 빈이 생성되도록 선언 가능
* 웹 기능 향상
  * @RestController 추가 : @ResponseBody를 붙이지 않아도 된다.
  * AsyncRestTemplate 추가 : RestTemplate에 대한 새로운 비동기식 구현으로 응답을 반환하고 연산이 끝났을 때 콜백을 호출할 수 있다.
* JEE 스펙 지원 : JMS 2.0, JTA 1.2, JPA 2.1, 빈 Validation 1.1 등
* 테스트 향상
  * 테스트에 사용되는 애너테이션들을 meta-annotation으로 사용할 수 있다.
  * @ActiveProfiles를 통해 특정 프로파일을 활성화 시킬 수 있다.
  * SocketUtils를 통해 localhost의 TCP, UDP 포트를 스캔할 수 있다.
  * org.springframework.mock.web 패키지에 있는 테스트 프레임워크는 서블릿 3.0 api 기반으로 동작한다.
