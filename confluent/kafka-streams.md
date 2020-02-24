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

## Etc

* Printed
