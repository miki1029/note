## Configurations

* StreamsConfig
* <https://docs.confluent.io/current/streams/developer-guide/config-streams.html>

### 필수 설정

* application.id : 전체 클러스터에서 고유한 값
  * default로 `클라이언트 ID 접두사`로 사용됨 : 카프카에 연결하는 클라이언트를 고유하게 식별하는 사용자 정의 값
  * default로 `그룹 ID`로 사용됨 : 동일한 토픽을 읽는 컨슈머 그룹의 구성원을 관리
* bootstrap.servers

## Serde

* Serdes

## Store

* <https://docs.confluent.io/current/streams/architecture.html#state>
* <https://docs.confluent.io/current/streams/developer-guide/dsl-api.html#streams-developer-guide-dsl-transformations-stateful>
* <https://docs.confluent.io/current/streams/architecture.html#fault-tolerance>
* <https://docs.confluent.io/current/streams/developer-guide/config-streams.html#num-standby-replicas>
* <https://bistros.tistory.com/entry/StreamsBuilder%EC%9D%98-table-%EB%A9%94%EC%86%8C%EB%93%9C%EB%8A%94-%ED%95%AD%EC%83%81-statestore%EC%99%80-changelog-%ED%86%A0%ED%94%BD%EC%9D%84-%EB%A7%8C%EB%93%A0%EB%8B%A4>
* 주요 클래스
  * Stores -> StoreSupplier : store의 유형 정적 팩토리 메소드
  * Materialized : 고수준 DSL을 사용하는 경우 주로 사용
  * StoreBuilder : 저수준 API를 사용하는 경우 주로 사용
* 변경 로그 토픽
  * withLoggingDisabled() : 로깅을 비활성화하며 내결함성 및 복구 기능도 제거
  * withLoggingEnabled(Map<String, String> config) : 변경 로그 토픽 설정
    * retention.ms
    * retention.bytes
    * cleanup.policy : 이 경우 특별하게 compact가 기본 값이다. 필요시 compact,delete 복수 설정 가능
  * 카프카 스트림즈가 자동으로 생성함
  * compacted topic

## Partition

* StreamPartitioner
* KStream.through

## Join

* ValueJoiner
* JoinWindows
* 조인하는 토픽은 서로 파티션 수가 같아야 한다.

## timestamp

* <https://github.com/miki1029/note/blob/master/confluent/kafka-config.md#timestamp-%EC%84%A4%EC%A0%95>
* 타임스탬프 처리 시맨틱
  * 이벤트 시간(CreateTime) : producer에서 보낸 시간
  * 인제스트 시간(LogAppendTime) : broker에서 설정한 시간
  * 처리 시간 : streams에서 소비한 시간(wall-clock time)
* ConsumerRecord의 메타데이터에는 topic 설정에 따라 이벤트 시간 또는 인제스트 시간이 포함된다.
* 레코드 value에 내장된 타임스탬프로 작업을 해야 하는 경우 사용자 정의 TimestampExtractor를 구현해야 한다.
* 인터페이스, 추상 클래스
  * TimestampExtractor : 최상위 인터페이스
  * ExtractRecordMetadataTimestamp : ConsumerRecord에서 타임스탬프를 추출하는 핵심 기능 추상 클래스
    * 보통 onInvalidTimestamp의 구현에 따라 구현 클래스가 달라짐
* 구현 클래스
  * ExtractRecordMetadataTimestamp
    * FailOnInvalidTimestamp
    * LogAndSinkOnInvalidTimestamp
    * UsePreviousTimeOnInvalidTimestamp
  * WallclockTimestampExtractor : System.currentTimeMillis()로 처리 시간 시맨틱을 제공하며 ConsumerRecord에서 타임 스탬프를 추출하지 않는다.
* config
  * default.timestamp.extractor : (default) FailOnInvalidTimestamp.class
  * Consumed.with(...).withTimestampExtractor(...)

### Spring Cloud Stream

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_state_store>

## Etc

* Printed
