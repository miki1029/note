### Pod, Container 관리

* Pod 이름을 호스트 이름으로 사용
* Pod 내부의 컨테이너들은 동일한 리눅스 네임스페이스 세트를 공유한다.
  * 동일한 네트워크 및 UTS 네임스페이스
  * 같은 호스트 이름 및 네트워크 인터페이스
  * 동일한 IPC 네임스페이스
  * 동일한 PID 네임스페이스(기본 비활성화)
  * localhost 공유
  * 포트 충돌이 나면 안됨
* 사이드카 컨테이너
  * 로그 로테이션 및 수집
  * 데이터 프로세서
  * 통신 어댑터

### livenes probe

* probe 종류
  * HTTP GET probe : check 2xx or 3xx
  * TCP socket probe : check TCP port
  * Exec probe : check 0
* 종료 코드
  * 137 = 128 + 9(SIGKILL)
  * 143 = 128 + 15(SIGTERM)

```yml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-liveness
spec:
  containers:
  - image: luksa/kubia-unhealthy
    name: kubia
    livenessProbe:
      httpGet:
        path: /
        port: 8080
      initialDelaySeconds: 15 # 첫 번째 프로브 실행 전 지연 / 시작 시간에 고려하여 설정
```

### Replication Controller

* 필수 요소
  * pod label selector
  * pod template
  * replicas 복제본 수
  * label selector와 pod template의 변경은 기존 pod에 영향을 미치지 않는다.
* Node down 시뮬레이션
  * sudo ifconfig eth0 down
  * k get node # NotReady -> Unknown -> pod 생성

### Secret

* 환경 변수로 컨테이너에 전달 가능
* 볼륨의 파일로 노출 가능
* automountService-AccountToken 필드 또는 pod이 사용하는 서비스 계정 설정으로 비활성화 가능

```
$ k exec <pod-name> -- ls /var/run/secrets/kubernetes.io/serviceaccount                                                                                
ca.crt
namespace
token
```
