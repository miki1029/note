![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F22107F505693B43E29E7AD)

빈 사용 준비

1. 인스턴스화
1. 프로퍼티 할당
1. BeanNameAware의 setBeanName(String name)
1. BeanClassLoaderAware의 setBeanClassLoader(ClassLoader classLoader)
1. BeanFactoryAware의 setBeanFactory(BeanFactory beanFactory)
1. ApplicationContextAware의 setApplicationContext(ApplicationContext applicationContext)
1. (다른 빈) BeanPostProcessor의 postProcessBeforeInitialization(Object bean, String beanName)
1. @PostConstruct 적용 메소드 호출
1. InitializingBean의 afterPropertiesSet()
1. init-method 커스텀 초기화 메소드 호출(xml)
1. (다른 빈) BeanPostProcessor의 postProcessAfterInitialization(Object bean, String beanName)

컨테이너 종료

1. @PreDestroy 적용 메소드 호출
1. DisposableBean의 destory()
1. destroy-method 커스텀 소멸 메소드 호출(xml)

참고

* IoC 컨테이너 : <https://blog.outsider.ne.kr/776>
* 빈 생명주기 관리 : <http://javaslave.tistory.com/48>
* BeanPostProcessor 사용 예 : <http://whiteship.tistory.com/561>
