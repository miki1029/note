# Spring Cloud OpenFeign

* <https://spring.io/projects/spring-cloud-openfeign>
* <https://cloud.spring.io/spring-cloud-openfeign/reference/html/>
* <https://woowabros.github.io/experience/2019/05/29/feign.html>
* <https://supawer0728.github.io/2018/03/11/Spring-Cloud-Feign/>

# Spring Cloud Stream

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.0.RELEASE/reference/html/>

## Spring Cloud Stream Kafka

### Consuming specific partition

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#spring-cloud-stream-overview-configuring-input-bindings-partitioning>
* 테스트시 용이한 설정, production에서는 kafka가 rebalance를 하도록 설정하는 것이 좋음

```yaml
spring:
  cloud:
    stream:
      bindings:
        <bindingName>:
          consumer:
            partitioned: true
      instance-index: 0 # 파티션 번호
      instance-count: 9 # 총 파티션 수
      kafka:
        bindings:
          <bindingName>:
            consumer:
              auto-rebalance-enabled: false
```

### Consuming Batches

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_consuming_batches>

```yaml
spring:
  cloud:
    stream:
      bindings:
        <bindngName>:
          consumer:
            batch-mode: true
      kafka:
        bindings:
          <bindingName>:
            consumer:
              configuration:
                max.poll.records: 1000
                fetch.min.bytes: 1
                fetch.max.wait.ms: 500
```

#### Customize failed message logging

* 메시지 처리 실패시 배치 메시지에 대한 전체 로그가 찍히기 때문에 로그가 매우 길어지게 된다. 따라서 필요한 정보만 출력하도록 수정할 필요가 있다.
* exception이 발생한 곳, error channel의 loggingHandler, batchErrorHandler 이렇게 세 곳에서 로그를 출력한다.

1. exception이 발생한 곳 : spring이 exception을 `IntegrationUtils.wrapInHandlingExceptionIfNecessary`를 통해서 `MessageHandlingException`으로 감싸게 된다. 이 과정에서 전체 메시지가 exception에 포함되어 로그가 매우 길어지게 된다. 이 때 메시지를 customize하고 싶다면 아래와 같이 임의의 message를 포함하는 `MessageHandlingException`으로 감싸서 던지면 된다.

```java
// build customPayload
try {
    // 메시지 처리 코드
} catch (Exception e) {
    throw new MessageHandlingException(new GenericMessage<>(customPayload), e);
}
```

2. error channel의 loggingHandler : error channel에서는 loggingHandler bean을 통해서 로그를 찍게 된다. 그런데 이 bean이 `DefaultConfiguringBeanFactoryPostProcessor.registerErrorChannel`에서 직접 생성하기 때문에 생성시 프로퍼티를 주입할 수가 없다. 따라서 애플리케이션의 configuration class에서 아래와 같이 설정했다.

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

3. batchErrorHandler : spring cloud stream의 batch 모드에서는 기본적으로 실패에 대한 재처리를 하지 않기 때문에 `BatchLoggingErrorHandler`가 default로 등록된다. 즉 batchErrorHandler를 다른 것으로 등록해주면 실패 메시지에 대한 로그를 찍지 않거나 customize할 수 있다.(아래 Support retry 참고)

4. debug level : 로그 레벨이 DEBUG 이하일 경우 추가적인 로깅이 더 찍히게 되는데, 아래 패키지를 INFO 이상으로 설정해주면 된다.

```yaml
logging:
  level:
    org.springframework.cloud.stream.messaging.DirectWithAttributesChannel: INFO
    org.springframework.cloud.stream.function.FunctionConfiguration: INFO
    org.springframework.cloud.stream.binder.BinderErrorChannel: INFO
    org.springframework.cloud.function.context.catalog.BeanFactoryAwareFunctionRegistry: INFO
    org.springframework.integration.channel.PublishSubscribeChannel: INFO
    org.springframework.integration.handler.LoggingHandler: INFO
    org.springframework.integration.handler.BridgeHandler: INFO
```

#### Support retry

* spring cloud stream의 batch 모드에서는 기본적으로 retry를 지원하지 않는다. retry를 지원하게 하려면 아래와 같이 `ListenerContainerCustomizer`를 사용하여 batchErrorHandler를 설정할 수 있다. 이 때 spring kafka에서 지원하는 `SeekToCurrentBatchErrorHandler`를 사용할 수 있다. 이 핸들러는 배치 처리 실패시 배치의 첫 오프셋으로 되돌린다. 추가적으로 backOff를 설정하면 각 retry 시도마다 sleep을 할 수 있다.

```java
@Bean
public ListenerContainerCustomizer<AbstractMessageListenerContainer<KeyType, ValueType>> listenerContainerCustomizer() {
    return (container, dest, group) -> {
        SeekToCurrentBatchErrorHandler seekToCurrentBatchErrorHandler = new SeekToCurrentBatchErrorHandler();
        seekToCurrentBatchErrorHandler.setBackOff(new FixedBackOff());
        container.setBatchErrorHandler(seekToCurrentBatchErrorHandler);
    };
}
```

#### Set maxAttempts for container stop

* 위에 설명했듯이 retry를 지원하지 않기 때문에 maxAttempts=1(재시도 하지 않음)이 강제적으로 설정된다. batchErrorHandler를 통한 retry는 maxAttempts 설정과 무관하게 동작한다. 따라서 maxAttempts를 구현하려면 batchErrorHandler를 직접 구현해야 한다. spring kafka에서는 `ContainerStoppingBatchErrorHandler`라는 클래스를 통해서 실패시 즉시 종료하는 핸들러를 지원한다. `SeekToCurrentBatchErrorHandler`와 `ContainerStoppingBatchErrorHandler`를 조합한 임의의 클래스를 구현하면 retry + maxAttempts 를 구현할 수 있다.

#### Auto commit

* auto commit이 활성화 되어 있다면 batch consume 하기 전에 이전 오프셋을 알아서 커밋하기 때문에 크게 신경쓰지 않아도 된다. 다만 retry를 별도로 설정하지 않으면 메시지 처리가 실패해도 auto commit이 되니 그 점만 주의하면 된다.
* `KafkaMessageListenerContainer.pollAndInvoke`
