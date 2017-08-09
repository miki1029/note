# 스프링 모듈

![spring module](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/images/spring-overview.png.pagespeed.ce.XVe1noRCMt.png)

**Core Container**

* 빈 생성, 설정, 처리 방법 관리
* 빈 팩토리 확인
* 애플리케이션 컨텍스트 구현체
* 이메일, JNDI 액세스, EJB 통합, 스케줄링 등

**AOP/Aspects**

* 애플리케이션 전체에 걸친 관심사(트랜잭션, 보안 등)
* 객체 간의 결합도를 낮춘다.

**Data Access/Integration**

* Hibernate, Java Persistence, iBATIS SQL Maps 프레임와크와 연결 고리 제공
* JMS(Java Message Service) : 다른 애플리케이션과의 비동기식 통합
* OXM(Object-XML Mapping)

**Web/Remoting**

* MVC 프레임워크
* RMI(Remote Method Invocation), Hessian, Burlap, JAX-WS, HTTP 호출자

**Instrumentation**

* JVM에 에이전트를 추가하는 기능
* 톰캣용 위빙 에이전트(weaving agent)

**Test**

* JNDI, Servlet, Portlet 모의 객체 구현
* 통합 테스트

# 스프링 포트폴리오

**Spring Web Flow**

* <http://projects.spring.io/spring-webflow>
* 목표를 향해 사용자를 안내하는 대화형, 흐름 기반 웹 애플리케이션 구축을 지원하기 위한 MVC 프레임워크

**Spring Web Service**

* <http://docs.spring.io/spring-ws/site>
* not contract-last but contract-first

**Spring Security**

* <http://projects.spring.io/spring-security>
* 선언적 보안 매커니즘
* Spring AOP를 이용하여 구현됨

**Spring Integration**

* <http://projects.spring.io/spring-integration>
* 몇 가지 공통적인 통합 패턴의 구현체를 스프링의 선언적 방식으로 제공

**Spring Batch**

* <http://projects.spring.io/spring-batch>
* 데이터 일괄 작업

**Spring Data**

* DB와의 구동
* JPA, NoSQL

**Spring Social**

* <http://spring.io/guides/gs/accessing-facebook>
* <http://spring.io/guides/gs/accessing-twitter>
* Facebook, Twitter connect

**Spring Mobile**

* 모바일 웹 애플리케이션 개발을 지원하는 스프링 MVC의 확장 기능

**Spring Android**

* <http://projects.spring.io/spring-android>

**Spring Boot**

* 스프링 그 자체를 간소화
* automatic configuration
* starter project : 프로젝트 빌드 파일의 크기를 줄인다.
