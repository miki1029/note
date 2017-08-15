## Property Source

* @PropertySource : 프로퍼티 파일을 등록
* Environment : 프로퍼티 정보를 가져올 수 있다.

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

Environment 프로퍼티 관련 메소드

* boolean containsProperty(String key)
* String getProperty(String key)
* String getProperty(String key, String defaultValue)
* T getProperty(String key, Class<T> type)
* T getProperty(String key, Class<T> type, T defaultValue)
* String getRequiredProperty(String key) : 없으면 IllegalStateException
* T getRequiredProperty(String key, Class<T> type)
* Class<T>getPropertyAsClass(String key, Class<T> type)

Environment 프로파일 관련 메소드

* String[] getActiveProfiles() : 활성화된 프로파일 명 배열
* String[] getDefaultProfiles() : 기본 프로파일 명 배열
* boolean acceptsProfiles(String... profiles) : 환경에 주어진 프로파일을 지원하면 true

## Property Placeholder

* 플레이스홀더 값을 사용하려면 Placeholder Configurer 빈 설정을 추가해야 한다.

**Plcaceholder Configurer 빈 설정**

* PropertyPlaceholderConfigurer
* PropertySourcesPlaceholderConfigurer : Environment와 Property source set에 대한 플레이스홀더 처리 포함

@PropertySource 사용

```java
@Configuration
@PropertySource("classpath:application.properties")
@ComponentScan
class ApplicationConfig {

    @Bean
    public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() {
        return new PropertySourcesPlaceholderConfigurer();
    }
}
```

직접 setLocation 사용 - 단 Environment에 등록되지는 않음

```java
@Configuration
@ComponentScan
class ApplicationConfig {

    @Bean
    public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() {
        PropertySourcesPlaceholderConfigurer c = new PropertySourcesPlaceholderConfigurer();
        c.setLocation(new ClassPathResource("application.properties"));
        // 프로퍼티 못가져올 때 IllegalArgumentException  억제
        // c.setIgnoreUnresolvablePlaceholders(true);
        return c;
    }

}
```

xml 방식

```xml
<context:property-placeholder />
```

<https://m.blog.naver.com/PostView.nhn?blogId=junsu60&logNo=220422158206&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F>

**Placeholder  사용**

* : 뒤에 디폴트 값 지정 가능(비워두면 빈 값 지정도 가능)
* 더보기 : <http://blog.codeleak.pl/2015/09/placeholders-support-in-value.html>

```java
@Data
@Component
public class MyBean {
    private String title;

    @Value("${mybean.name:Default}")
    private String name;

    @Value("${mybean.name:}")
    private String name2;

    public MyBean(@Value("${mybean.title}") String title) {
        this.title = title;
    }
}
```

```xml
<bean id="myBean" class="miki1029.MyBean" c:_title="${my.title}" />
```
