런타임에 값을 평가하는 방법

* Property placeholders
* SpEL (Spring Expression Language)


**Environment**

```java
@Configuration
@PropertySource("classpath:/miki1029/app.properties")
public class MyConfig {
	@Autowired
	private Environment env;
	
	@Bean
	public MyObject myObject() {
		return new MyObject(env.getProperty("my.property.name", "default value"));
	}
}
```

프로퍼티 관련

* boolean containsProperty(String key)
* String getProperty(String key)
* String getProperty(String key, String defaultValue)
* T getProperty(String key, Class<T> type)
* T getProperty(String key, Class<T> type, T defaultValue)
* String getRequiredProperty(String key) : 없으면 IllegalStateException
* T getRequiredProperty(String key, Class<T> type)
* Class<T>getPropertyAsClass(String key, Class<T> type)

프로파일 관련

* String[] getActiveProfiles() : 활성화된 프로파일 명 배열
* String[] getDefaultProfiles() : 기본 프로파일 명 배열
* boolean acceptsProfiles(String... profiles) : 환경에 주어진 프로파일을 지원하면 true

**Plcaceholder Configurer 빈 설정**

* PropertyPlaceholderConfigurer
* PropertySourcesPlaceholderConfigurer : Environment와 Property source set에 대한 플레이스홀더 처리 포함

```java
@Bean
public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() {
    return new PropertySourcesPlaceholderConfigurer();
}
```

```xml
<context:property-placeholder />
```

<https://m.blog.naver.com/PostView.nhn?blogId=junsu60&logNo=220422158206&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F>

**Placeholder  사용**

```java
@Data
@Component
public class MyBean {
    private String title;

    @Value("${mybean.name}")
    private String name;

    public MyBean(@Value("${mybean.title}") String title) {
        this.title = title;
    }
}
```

```xml
<bean id="myBean" class="miki1029.MyBean" c:_title="${my.title}" />
```
