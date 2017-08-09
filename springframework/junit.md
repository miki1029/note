StandardOutputStreamLog : <http://stefanbirkner.github.io/system-rules/index.html>

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes=AppConfig.class)
public class MyTest {
  @Rule
  public final SystemOutRule log = new SystemOutRule().enableLog();
  
  @Test
  public void test() {
    // system out logic
    assertEquals("something", log.getLog());
  }
}
```
