Bean scope

* Singleton : 오직 하나
* Prototype : 빈이 주입될 때마다, 컨텍스트에서 가져올 때마다 생성
* Session : Web Application에서 각 세션용으로 생성되는 하나
* Request : Web Application에서 각 요청용으로 생성되는 하나

Scope 선언

```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)

@Bean
@Scope("prototype")
```

```xml
<bean id="myBean" class="miki1029.MyBean" scope="prototype" />
```

Session

```java
@Bean
@Scope(value=WebApplicationContext.SCOPE_SESSION,
	proxyMode=ScopedProxyMode.INTERFACES)
public MySessionBean mySessionBean() { ... }

@Component
public class MyService {
	// ...
	
	@Autowired
	public void setMySessionBean(MySessionBean mySessionBean) {
		this.mySessionBean = mySessionBean;
	}
}
```

```xml
<!--인터페이스-->
<bean id="mySessionBean" class="miki1029.MySessionBean" scope="session">
	<aop:scoped-proxy proxy-target-class="false" />
</bean>
<!--구상클래스 (cglib)-->
<bean id="mySessionBean" class="miki1029.MySessionBean" scope="session">
	<aop:scoped-proxy />
</bean>
```

* MyService는 컨텍스트 로드시에 싱글톤 빈이 생성되지만 아직 세션이 없으므로 MySessionBean이 없다.
* 스프링은 이 때 프록시를 주입한다.
* 인터페이스 프록시 : ScopedProxyMode.INTERFACES
* 구상클래스 프록시 : ScopedProxyMode.TARGET_CLASS (cglib)
