# Concepts

* Streams Concepts : <https://docs.confluent.io/current/streams/concepts.html>
* Streams Architecture : <https://docs.confluent.io/current/streams/architecture.html>
* Streams Developer Guide
  * <https://docs.confluent.io/current/streams/developer-guide/index.html>
  * <https://kafka.apache.org/24/documentation/streams/developer-guide/>
* Log Compaction : <https://kafka.apache.org/documentation.html#compaction>
  * Hence log compaction is perfectly safe for a KTable (changelog stream) but it is a mistake for a KStream (record stream).

# Configurations

* <https://docs.confluent.io/current/installation/configuration/index.html>
* <https://kafka.apache.org/documentation/>

## Broker Configurations

* <https://docs.confluent.io/current/installation/configuration/broker-configs.html>

### 로그 설정

* <https://bigdatalab.tistory.com/10>
* log.dir : 카프카 로그 디렉토리
* 로그 롤링 (세그먼트)
 * 첫 번째 메시지의 타임스탬프가 기준
 * 단위가 작은 설정이 우선순위가 높다.
 * log.roll.ms
 * log.roll.hours : default 168
* 로그 세그먼트 삭제
 * 시간 단위 관리
  * 세그먼트에서 가장 큰 타임스탬프가 기준
  * 단위가 작은 설정이 우선순위가 높다.
  * log.retention.ms
  * log.retention.minutes
  * log.retention.hours : default 168
 * 용량 단위 관리
  * log.retention.bytes : 파티션 로그 최대 크기
  * log.segment.bytes : 세그먼트 로그 최대 크기 default 1GB (1073741824)

## Topic Configurations

* <https://docs.confluent.io/current/installation/configuration/topic-configs.html>
* <https://kafka.apache.org/documentation/#topicconfigs>



## Kafka Configurations

* producer
 * <https://docs.confluent.io/current/installation/configuration/producer-configs.html>
 * <https://kafka.apache.org/documentation/#producerconfigs>
* consumer
 * <https://docs.confluent.io/current/installation/configuration/consumer-configs.html>
 * <https://kafka.apache.org/documentation/#consumerconfigs>
* kr blog : <https://ujfish-tools.tistory.com/entry/kafka-centos-%ED%99%95%EC%9D%B8%EC%82%AC%ED%95%AD>
* <https://team-platform.tistory.com/32>

## Kafka Streams Configurations

* streams : <https://kafka.apache.org/24/documentation/streams/developer-guide/config-streams.html>
* serde : <https://docs.confluent.io/current/streams/developer-guide/datatypes.html>

## Spring Configurations

### Spring Kafka

* <https://docs.spring.io/spring-kafka/reference/html/>

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
