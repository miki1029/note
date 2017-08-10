런타임에 값을 평가하는 방법

* Property placeholders
* SpEL (Spring Expression Language)

## Property placeholders

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

Environment

* String getProperty(String key)
* String getProperty(String key, String defaultValue)
* T getProperty(String key, Class<T> type)
* T getProperty(String key, Class<T> type, T defaultValue)
