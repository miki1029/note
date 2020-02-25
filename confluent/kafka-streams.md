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

### Spring Cloud Stream

* <https://cloud.spring.io/spring-cloud-static/spring-cloud-stream-binder-kafka/3.0.1.RELEASE/reference/html/spring-cloud-stream-binder-kafka.html#_state_store>

## Etc

* Printed
