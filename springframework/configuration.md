## Configuration

Java Config

```java
@Configuration
@ComponentScan
@Import(AppConfig2.class)
public class AppConfig {}

@ComponentScan
@ComponentScan("miki1029")
@ComponentScan(basePackages="miki1029")
@ComponentScan(basePackages={"miki1029", "mini1029"})
@ComponentScan(basePackageClasses={MyComponent.class, PackageInfo.class})

@Import(AppConfig2.class)
@Import({AppConfig2.class, AppConfig3.class})
@ImportResource("classpath:appConfig.xml")

```

Xml Config

```xml
<context:component-scan base-package="miki1029" />
```

Test Java Config

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=AppConfig.class)
```

## Bean

자동 스프링 설정

```java
@ComponentScan
@ComponentScan("beanName")
@Named("beanName") // JSR-330

@Autowired
@Autowired(required=false)
@Inject
```

Java 빈 선언

```java
@Configuration
public class AppConfig {
	@Bean
	public MyBean myBean() {
		return new MyBean();
	}
	
	@Bean
	public MyBean2 myBean2() {
		// myBean() 에서 실제 메소드를 호출하지 않고 이미 만들어진 빈을 리턴해 준다.
		return new MyBean2(myBean());
	}
	
	@Bean
	// myBean 빈을 주입해 준다.
	public MyBean2 myBean2(MyBean myBean) {
		return new MyBean2(myBean);
	}
}

@Bean
@Bean(name="beanName")
```

XML 빈 선언

```xml
<bean class="miki1029.MyBean" /> <!-- beanName : miki1029.MyBean#0 -->
<bean id="myBean" class="miki1029.MyBean" />

<!--생성자 주입-->
<bean id="myBean2" class="miki1029.MyBean2">
	<constructor-arg ref="myBean">
</bean>
<bean id="myBean2" class="miki1029.MyBean2">
	<constructor-arg value="literal value">
	<constructor-arg><null/></constructor-arg>
	<constructor-arg>
		<list>
			<value>string value1</value>
			<ref bean="myBean"/>
		</list>
	</constructor-arg>
	<constructor-arg>
		<set>...</set>
	</constructor-arg>
	<property name="propertyName" ref="myBean" />
	<property name="propertyName">
		<list>...</list>
	</property>
</bean>

<!--paramName : 생성자 파라미터명-->
<bean id="myBean2" class="miki1029.MyBean2" c:paramName-ref="myBean" />
<bean id="myBean2" class="miki1029.MyBean2" c:paramName="literal value" />
<bean id="myBean2" class="miki1029.MyBean2" p:propertyName-ref="myBean" />
<bean id="myBean2" class="miki1029.MyBean2" p:propertyName="literal value" />

<!--_0 : 첫 번째 파라미터-->
<bean id="myBean2" class="miki1029.MyBean2" c:_0-ref="myBean" />
<bean id="myBean2" class="miki1029.MyBean2" c:_0="literal value" />

<!--파라미터 하나일 때-->
<bean id="myBean2" class="miki1029.MyBean2" c:_-ref="myBean" />
<bean id="myBean2" class="miki1029.MyBean2" c:_-="literal value" />

<!--util-namespace : list bean-->
<util:list id="myList">
	<value>value1</value>
</util:list>
```

**util-namespace**

빈으로 만들거나 노출시킴

* list : java.util.List
* set : java.util.Set
* map : java.util.Map
* properties : java.util.Properties
* property-path : 빈 프로퍼티 참조
* constant : public static 필드 참조

