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
