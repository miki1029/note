# Consumer Groups

* 컨슈머 그룹 reset offset

```
$ kafka-consumer-groups --bootstrap-server <broker1,broker2> --topic <topicName> --group <groupName> --reset-offsets --to-earliest --execute
```

# Configurations

## Kafka Configurations

producer  
https://kafka.apache.org/documentation/#producerconfigs

consumer  
https://kafka.apache.org/documentation/#consumerconfigs

### Reference

* <https://ujfish-tools.tistory.com/entry/kafka-centos-%ED%99%95%EC%9D%B8%EC%82%AC%ED%95%AD>

## Spring Configurations

### Spring Cloud Stream

spring.cloud.stream.default  
spring.cloud.stream.bindings.<bindingName>  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_common_binding_properties

spring.cloud.stream.default.consumer  
spring.cloud.stream.bindings.<bindingName>.consumer  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_consumer_properties

spring.cloud.stream.default.producer  
spring.cloud.stream.bindings.<bindingName>.producer  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_producer_properties

### Spring Cloud Stream Kafka

spring.cloud.stream.kafka.binder  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_kafka_binder_properties

spring.cloud.stream.kafka.default.consumer  
spring.cloud.stream.kafka.bindings.<channelName>.consumer  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#kafka-consumer-properties

spring.cloud.stream.kafka.default.producer  
spring.cloud.stream.kafka.bindings.<channelName>.producer  
https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#kafka-producer-properties
