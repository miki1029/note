# Istio

<https://istio.io/latest/docs/concepts/what-is-istio/>

* Service Mesh 요구사항
  * 검색
  * 로드 밸런싱
  * 장애 복구
  * 메트릭 및 모니터링
  * 서비스 간 인증

* Istio 사용하는 이유
  * 트래픽 부하 자동 분산
  * 풍부한 라우팅 규칙, retry, failover, fault injection
  * 엑세스 제어, rate limit, quota
  * 모든 ingress, egress 트래픽에 대한 메트릭, 로그, 트레이싱
  * ID 기반 인증 및 권한 부여
  
* 핵심 기능
  * 트래픽 관리
  * 보안
  * Observability

## 트래픽 관리

* <https://istio.io/latest/docs/concepts/traffic-management/>
* 서비스간 트래픽 흐름과 API 호출 제어
* circuit breaker, timeout, retry
* 퍼센트 기반의 A/B test, canary rollout, staged rollout

## 보안

* <https://istio.io/latest/docs/concepts/security/>
* 보안 통신 채널 지원
* 대규모 서비스의 인증, 권한, 암호화
* 애플리케이션 변경 없이 일관되게 적용

## Observability

* <https://istio.io/latest/docs/concepts/observability/>
* 강력한 추적, 모니터링, 로깅
* 
