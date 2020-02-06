# Consumer Groups

* 컨슈머 그룹 reset offset

```
$ kafka-consumer-groups --bootstrap-server <broker1,broker2> --topic <topicName> --group <groupName> --reset-offsets --to-earliest --execute
```

# Configurations

## Kafka Configurations

* producer : <https://kafka.apache.org/documentation/#producerconfigs>
* consumer : <https://kafka.apache.org/documentation/#consumerconfigs>

### Reference

* <https://ujfish-tools.tistory.com/entry/kafka-centos-%ED%99%95%EC%9D%B8%EC%82%AC%ED%95%AD>

## Spring Configurations

### Spring Cloud Stream

#### spring.cloud.stream.bindings.\<bindingName\>

* spring.cloud.stream.default
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_common_binding_properties>

#### spring.cloud.stream.bindings.\<bindingName\>.producer

* spring.cloud.stream.default.producer
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_producer_properties>

#### spring.cloud.stream.bindings.\<bindingName\>.consumer

* spring.cloud.stream.default.consumer
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/spring-cloud-stream.html#_consumer_properties>

### Spring Cloud Stream Kafka

#### spring.cloud.stream.kafka.binder

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_kafka_binder_properties>

#### spring.cloud.stream.kafka.bindings.\<bindingName\>.producer

* spring.cloud.stream.kafka.default.producer
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#kafka-producer-properties>

#### spring.cloud.stream.kafka.bindings.\<bindingName\>.consumer

* spring.cloud.stream.kafka.default.consumer
* https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#kafka-consumer-properties

### Spring Cloud Stream Kafka Streams

#### spring.cloud.stream.kafka.streams.binder

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_kafka_streams_binder_properties>

#### spring.cloud.stream.kafka.streams.bindings.\<bindingName\>.producer

* spring.cloud.stream.kafka.streams.default.producer
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_kafka_streams_producer_properties>

#### spring.cloud.stream.kafka.streams.bindings.\<bindingName\>.consumer

* spring.cloud.stream.kafka.streams.default.consumer
* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_kafka_streams_consumer_properties>
