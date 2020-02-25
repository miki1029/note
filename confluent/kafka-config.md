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
  * 정책 관리
    * log.cleanup.policy
      * compact : 동일한 키가 있을 때 이전 레코드들을 삭제한다.
      * delete : (default) 일정 시간 경과 후 레코드들을 삭제한다.

### timestamp 설정

* log.message.timestamp.type
  * CreateTime : (default) 이벤트 시간 / producer가 보낸 timestamp를 사용
  * LogAppendTime : 처리 시간 / broker 기준

## Topic Configurations

* <https://docs.confluent.io/current/installation/configuration/topic-configs.html>
* <https://kafka.apache.org/documentation/#topicconfigs>
* Broker Configurations 설정의 키에서 `log` prefix를 제거하면 거의 대부분 동일하다.


## Kafka Configurations

* <https://ujfish-tools.tistory.com/entry/kafka-centos-%ED%99%95%EC%9D%B8%EC%82%AC%ED%95%AD>
* <https://team-platform.tistory.com/32>

### producer

* <https://docs.confluent.io/current/installation/configuration/producer-configs.html>
* <https://kafka.apache.org/documentation/#producerconfigs>
* bootstrap.servers
* key.serializer
* value.serializer
* acks
  * all : 모든 팔로워가 커밋할 때까지 대기
  * 1 : 리더의 커밋만 대기
  * 0 : 커밋을 기다리지 않음
* retries : 재시도 횟수
  * 레코드 순서가 중요한 경우 `max.in.flight.requests.per.connection`을 1로 설정
* compression.type
  * none, gzip, snappy, lz4, zstd
* partitioner.class
  * Partitioner 인터페이스를 구현하는 클래스의 이름
* batch.size
* linger.ms

### consumer

* <https://docs.confluent.io/current/installation/configuration/consumer-configs.html>
* <https://kafka.apache.org/documentation/#consumerconfigs>
* 컨슈머 인스턴스들의 총 스레드 수는 파티션 수를 넘지 않아야 한다. (유휴 상태가 됨)
* auto.offset.reset : 오프셋이 없을 때 초기화 전략
  * earliest
  * latest : (default)
  * none : 오프셋이 없으면 브로커가 컨슈머에게 예외를 발생시킨다.
* enable.auto.commit : (default true)
* auto.commit.interval.ms : (default 5000)
* group.id

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
