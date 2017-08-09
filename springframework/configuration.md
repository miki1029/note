## Configuration

Java Config

```java
@Configuration
@ComponentScan
public class AppConfig {}

@ComponentScan
@ComponentScan("miki1029")
@ComponentScan(basePackages="miki1029")
@ComponentScan(basePackages={"miki1029", "mini1029"})
@ComponentScan(basePackageClasses={MyComponent.class, PackageInfo.class})
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

```java
@ComponentScan
@ComponentScan("beanName")
@Named("beanName") // JSR-330

@Autowired
@Autowired(required=false)
@Inject
```
