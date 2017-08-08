Application Context

* AnnotationConfigApplicationContext
  * 자바 기반 설정
* ClassPathXmlApplicationContext
  * 클래스패스에 위치한 xml 기반 설정
* FileSystemXmlApplicationContext
  * 파일 경로로 지정된 xml 기반 설정

Web Application Context

* AnnotationConfigWebApplicationContext
  * 자바 기반 설정
* XmlWebApplicationContext
  * xml 기반 설정


```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
ApplicationContext context = new ClassPathXmlApplicationContext("context.xml");
ApplicationContext context = new FileSystemXmlApplicationContext("/home/user/context.xml");
```
