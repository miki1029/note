## 프로파일 설정

Java Configuration

```java
// class
@Configuration
@Profile("dev")

// method
@Bean
@Profile("dev")
```

XML Configuration

```xml
<!--전체 설정-->
<beans xmlns="..." ... profile="dev">...</beans>

<!--부분 설정-->
<beans xmlns="..." ...>
	<beans profile="dev">
		<bean .../>
	</beans>
</beans>
```

## 프로파일 활성화

1. spring.profiles.active 에 설정된 값
2. spring.profiles.default 에 설정된 값
3. 활성화 프로파일이 없으면 프로파일에 정의되지 않은 빈만 만들어진다.
4. ,로 구분된 여러 프로파일을 설정할 수 있다.

프로파일 프로퍼티 설정

* default : DispatcherServlet에 초기화된 파라미터
* default : 웹 애플리케이션 컨텍스트 파라미터
* active : JDNI 엔트리
* active : 환경 변수
* active : JVM 시스템 프로퍼티
* active : 통합 테스트 클래스에서 @ActiveProfiles 애너테이션 사용

```xml
<!--web.xml-->

<!--Web Application Context-->
<context-param>
	<param-name>spring.profiles.default</param-name>
	<param-value>dev</param-value>
</context-param>

<!--DispatcherServlet-->
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>spring.profiles.default</param-name>
		<param-value>dev</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
```

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes={AppConfig.class})
@ActiveProfiles("dev")
```

## 조건부 빈

```java
@Bean
@Conditional(MyCondition.class)
```

```java
public class MyCondition implements Condition {
	@Override
	public boolean matches(ConditionalContext context, AnnotatedTypeMetadata metadata) {
		Environment env = context.getEnvironment();
		return env.containsProperty("my");
	}
}
```

스프링 4에서는 @Profile이 @Conditional과 Condition으로 리팩토링되었다.
