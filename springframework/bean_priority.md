기본 빈 지정

```java
@Component
@Primary

@Bean
@Primary
```

```xml
<bean id="myBean" class="miki1029.MyBean" primary="true" />
```

수식자 지정

```java
// class
@Component
@Qualifier("myBean")

// method
@Bean
@Qualifier("myBean")

// inject
@Autowired
@Qualifier("myBean")
```

맞춤형 수식자 애너테이션

* 문자열보다 type-safe
* 여러 수식자 애너테이션 동시 사용 가능

```java
@Target({ElementType.CONSTRUCTOR, ElementType.FIELD, ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface MyQualifier { }
```
