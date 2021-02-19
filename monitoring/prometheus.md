# 모니터링

모니터링의 의미

* 알림
* 디버깅
* 추세 파악(tranding)
* 플러밍(plumbing)

모니터링의 범주

* 프로파일링
  * 제한된 기간의 일부 컨텍스트
  * Tcpdump
  * debug builds
  * eBPF(enhanced Berkeley Packet Filters) : 파일시스템부터 네트워크 기호까지 커널 이벤트에 대해 상세하기 프로파일링 [link](http://brendangregg.com/blog/2019-01-01/learn-ebpf-tracing.html)
* 트레이싱
  * 관심 기능을 통과하는 특정 이벤트에만 집중
  * stack trace
  * HTTP 요청 중 하나를 샘플링하여 DB 등 백엔드와 통신하는 데 소비되는 시간
  * 분산 트레이싱(distributed tracing)
    * 원격 프로시저 호출에서 한 프로세스에서 다른 프로세스로 전달되는 요청에 고유한 ID를 추가
    * OpenZipkin, Jaeger
* 로깅
  * 제한된 이벤트 집합을 살펴보고 컨텍스트 일부를 기록
  * 로그의 종류
    * 트랜잭션 로그(transaction logs)
      * 영원히 안전하게 보관해야 하는 중요한 비즈니스 기록
      * 비용 관련이나 사용자가 직접 사용하는 주요 기능
    * 요청 로그(request logs)
    * 애플리케이션 로그
    * 디버그 로그
  * 트랜잭션 로그와 디버그 로그는 저장소를 분리해서 관리해야 함
  * ELK stack, Graylog
* 메트릭
  * 다양한 유형의 이벤트에 대해 시간에 따른 집계를 추적
  * 자원 사용을 정상적으로 유지하려면, 추적하는 메트릭의 개수를 제한해야 한다.
  * 프로세스당 1만 개 정도
  * 수신된 HTTP 요청 횟수, 요청을 처리하는데 걸린 시간, 현재 진행 중인 요청 수

# Prometheus

![prometheus arthitecture](https://prometheus.io/assets/architecture.png)

## 계측

* \_total, \_count, \_sum, \_bucket

### 카운터

* 메소드 : inc
* \_total

### 게이지

* 메소드 : inc, dec, set

### 서머리

* 메소드 : observe
* \_count, \_sum
* irate(xxx_count\[5m\]) : 초당 요청 수
* irate(xxx_sum\[5m\]) : 초당 응답 시간
* rate(xxx_sum\[5m\]) / rate(xxx_count\[5m\]) : 마지막 5분에 대한 평균 대기시간

### 히스토그램

* 300밀리초의 0.95분위수라고 한다면 요청의 95%는 300초 미만의 시간이 걸린다.
* 메소드 : observe, time
* \_bucket
* 버킷 집합 : 1밀리초, 10밀리초, 25밀리초
* 각 버킷에 속하는 이벤트 수를 추적
* histogram_quantile : 버킷들로부터 분위수 계산
* histogram_quantile(0.95, rate(xxx_bucket\[1m\]))
