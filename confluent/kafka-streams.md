## Concepts

### State

* <https://docs.confluent.io/current/streams/architecture.html#state>
* <https://docs.confluent.io/current/streams/developer-guide/dsl-api.html#streams-developer-guide-dsl-transformations-stateful>
* <https://docs.confluent.io/current/streams/architecture.html#fault-tolerance>
* <https://docs.confluent.io/current/streams/developer-guide/config-streams.html#num-standby-replicas>
* <https://bistros.tistory.com/entry/StreamsBuilder%EC%9D%98-table-%EB%A9%94%EC%86%8C%EB%93%9C%EB%8A%94-%ED%95%AD%EC%83%81-statestore%EC%99%80-changelog-%ED%86%A0%ED%94%BD%EC%9D%84-%EB%A7%8C%EB%93%A0%EB%8B%A4>
* `KStream.transformValues()`
  * 의미상으로 `KStream.mapValues()`와 동일하지만
  * StateStore 인스턴스에 접근해서 작업을 완료한다.
  * `punctuate()` 메소드를 통해 정기적인 간격으로 작업이 수행되도록 예약이 가능하다.
  * 오프셋 커밋은 언제 이루어질까??
  * `ValueTransformer` 구현시 init에서 stateStore를 가져오고 transform에서 store를 이용해 누적 데이터 등으로 변환하면 된다.
* 주요 클래스
  * Stores -> StoreSupplier : store의 유형 정적 팩토리 메소드
  * Materialized : 고수준 DSL을 사용하는 경우 주로 사용
  * StoreBuilder : 저수준 API를 사용하는 경우 주로 사용
* 변경 로그 토픽 : State에 쓰기 전에 변경로그 토픽에 먼저 반영 후 기록하기 때문에 failover가 가능하다.
  * withLoggingDisabled() : 로깅을 비활성화하며 내결함성 및 복구 기능도 제거
  * withLoggingEnabled(Map<String, String> config) : 변경 로그 토픽 설정
    * retention.ms
    * retention.bytes
    * cleanup.policy : 이 경우 특별하게 compact가 기본 값이다. 필요시 compact,delete 복수 설정 가능
  * 카프카 스트림즈가 자동으로 생성함
  * compacted topic

### Repartition

* `KStream.through("토픽명", Produeced.with(keySerde, valueSerde, streamPartitioner))` : 새로운 중간 토픽을 만들어 데이터를 리파티셔닝한다.
  * `StreamPartitioner` : 기존의 카프카 키를 통해서 파티셔닝을 하는 것이 아닌 임의의 파티셔닝 키를 사용하고자 할 때

### Join

* ValueJoiner
* JoinWindows
* 조인하는 토픽은 서로 파티션 수가 같아야 한다.

```
p1Stream.join(p2Stream,
              (p1, p2) -> new CP(p1, p2),
              JoinWindows.of(60 * 1000 * 20), // sliding window
              Joined.with(stringSerde, pSerde, pSerde));
```

### Timestamp

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

### Aggregation

* `KStream.groupBy` -> `KGroupedStream.reduce` -> `KTable`
* `KStream.groupBy` -> `KGroupedStream.aggregate` -> `KTable`
  * groupBy : key가 바뀔 수 있으므로 repartition 발생
  * groupByKey : key가 바뀌지 않으므로 repartition 없음

### Window

* 세션 윈도
  * `KStream.groupBy` -> `KGroupedStream.windowedBy(SessionWindows.with(1000 * 20).until(1000 * 60 * 15))` -> `SessionWindowedKStream`
    * with : 비활성 간격
    * until : 유지 기간
* 텀블링 윈도
  * `KStream.groupBy` -> `KGroupedStream.windowedBy(TimeWindows.of(1000 * 20))` -> `TimeWindowedKStream`
* 슬라이딩 또는 호핑 윈도
  * 텀블링 윈도와 다른 점 : advanceBy 만큼 자주 업데이트하며 겹쳐진 데이터를 포함할 수 있다.
  * `KStream.groupBy` -> `KGroupedStream.windowedBy(TimeWindows.of(1000 * 20).advanceBy(1000 * 5).until(1000 * 60 * 15))` -> `TimeWindowedKStream`
    * until : @deprecated since 2.1. Use Materialized.retention or directly configure the retention in a store supplier and use Materialized.as(WindowBytesStoreSupplier).

## Configurations

* StreamsConfig
* <https://docs.confluent.io/current/streams/developer-guide/config-streams.html>

### 필수 설정

* application.id : 전체 클러스터에서 고유한 값
  * default로 `클라이언트 ID 접두사`로 사용됨 : 카프카에 연결하는 클라이언트를 고유하게 식별하는 사용자 정의 값
  * default로 `그룹 ID`로 사용됨 : 동일한 토픽을 읽는 컨슈머 그룹의 구성원을 관리
* bootstrap.servers

### KTable

* cache.max.bytes.buffering : (default 10485760 10MiB) 테이블 캐시 크기
* commit.interval.ms : (default 30000 milliseconds (at-least-once) / 100 milliseconds (exactly-once)) 커밋 주기

### Spring Cloud Stream

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_state_store>

## Etc

* Printed
