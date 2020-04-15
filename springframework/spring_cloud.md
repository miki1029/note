# Spring Cloud OpenFeign

* <https://spring.io/projects/spring-cloud-openfeign>
* <https://cloud.spring.io/spring-cloud-openfeign/reference/html/>
* <https://woowabros.github.io/experience/2019/05/29/feign.html>
* <https://supawer0728.github.io/2018/03/11/Spring-Cloud-Feign/>

# Spring Cloud Stream

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.0.RELEASE/reference/html/>

## Spring Cloud Stream Kafka

### Consuming Batches

* https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_consuming_batches

#### Customize failed message logging

* 메시지 처리 실패시 배치 메시지에 대한 전체 로그가 찍히기 때문에 로그가 매우 길어지게 된다. 따라서 필요한 정보만 출력하도록 수정할 필요가 있다.
* exception이 발생한 곳과 error channel 이렇게 두 곳에서 로그를 출력한다.

1. 메시지 처리 과정에서 예외 발생시 spring이 exception을 `IntegrationUtils.wrapInHandlingExceptionIfNecessary`를 통해서 `MessageHandlingException`으로 감싸게 된다. 이 과정에서 전체 메시지가 exception에 포함되어 로그가 매우 길어지게 된다. 이 때 메시지를 customize하고 싶다면 아래와 같이 임의의 message를 포함하는 `MessageHandlingException`으로 감싸서 던지면 된다.

```java
// build customPayload
try {
    // 메시지 처리 코드
} catch (Exception e) {
    throw new MessageHandlingException(new GenericMessage<>(customPayload), e);
}
```

2. error channel에서는 loggingHandler bean을 통해서 로그를 찍게 된다. 그런데 이 bean이 `DefaultConfiguringBeanFactoryPostProcessor.registerErrorChannel`에서 직접 생성하기 때문에 생성시 프로퍼티를 주입할 수가 없다. 따라서 애플리케이션의 configuration class에서 아래와 같이 설정했다.

```java
@Autowired
@Qualifier(IntegrationContextUtils.ERROR_LOGGER_BEAN_NAME + IntegrationConfigUtils.HANDLER_ALIAS_SUFFIX)
private LoggingHandler loggingHandler;

@PostConstruct
public void postConstruct() {
    loggingHandler.setLogExpression(new FunctionExpression<>(message -> {
        if (message instanceof ErrorMessage) {
            message = ((ErrorMessage) message).getOriginalMessage();
        }
        // build customMessage from message
        return customMessage;
    });
}
```

#### Support retry

* spring cloud stream의 batch 모드에서는 기본적으로 retry를 지원하지 않는다. retry를 지원하게 하려면 아래와 같이 `ListenerContainerCustomizer`를 사용하여 batchErrorHandler를 설정할 수 있다. 이 때 spring kafka에서 지원하는 `SeekToCurrentBatchErrorHandler`를 사용할 수 있다. 이 핸들러는 배치 처리 실패시 배치의 첫 오프셋으로 되돌린다.

```
@Bean
public ListenerContainerCustomizer<AbstractMessageListenerContainer<KeyType, ValueType>> listenerContainerCustomizer() {
    return (container, dest, group) -> {
        SeekToCurrentBatchErrorHandler seekToCurrentBatchErrorHandler = new SeekToCurrentBatchErrorHandler();
        seekToCurrentBatchErrorHandler.setBackOff(new FixedBackOff());
        container.setBatchErrorHandler(seekToCurrentBatchErrorHandler);
    };
}
```

#### Set maxAttempts for application failing

* 위에 설명했듯이 retry를 지원하지 않기 때문에 maxAttempts=1(재시도 하지 않음)이 강제적으로 설정된다. batchErrorHandler를 통한 retry는 maxAttempts 설정과 무관하게 동작한다. 따라서 maxAttempts를 설정하려면 별도의 과정을 통해 설정해야 한다.

#### Auto commit

