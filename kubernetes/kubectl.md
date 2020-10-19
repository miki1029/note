# Kubectl

## Reference

* <https://kubernetes.io/ko/docs/reference/>
* <https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/>

## 약어

```
k api-resources

k kubectl
rc replicationcontrollers
po pods
svc services
pvc persistentvolumeclaims
pv persistentvolumes
sc storageclasses
deploy deployments
rs replicasets
sts statefulesets
ing ingresses
no nodes node

--namespace -n
```

## 명령어

### config & 정보 출력

```
# 정보 출력
k config current-context
k config get-contexts
k cluster-info
k describe po [<pod-name>]
k describe no [<context-name>]
k describe no
k explain po.spec

# 현재 상태 변경
k config use-context <context-name>

# config 파일 수정
k config set-context <context-name> [--key=value]
k config set-context --current --namespace=default
k config set-context minikube --namespace=default
```

```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=jenkins" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 8080:8080
```

### get

```
k get all
k get ns
k get deployments
k get svc
k get po
k get pvc
k get pv
k get sc

# IP, NODE, NOMINATED NODE, READINESS GATES
k get po -o wide

# get pods options
k get po --all-namespaces
k get po my-pod -o yaml
k get po my-pod -o yaml --export
k get po --show-labels

# 라벨과 함께 조회
k get po -L <label1>[,<label2>,...]
k get no -L kubernetes.io/hostname

# 라벨로 조회
k get po -l <key>
k get po -l <key>=<value>
k get po -l <key>!=<value>
k get po -l '!<key>'
k get po -l '<key> in (<value1>[, <value2>, ...])
k get po -l '<key> notin (<value1>[, <value2>, ...])

# 라벨로 조회시 ,를 이용해 and 조건 구성 가능
k get po -l <key1>,<key2>

# get pod 이외의 예시
k get no -l <key>=<value>
k delete po -l <key>=<value>
```

### namespace

```
k create ns <namespace>
k delete ns <namespace>
```

### label

```
# 라벨 생성
k label <object> <pod> <key>=<value>
k label po <pod> <key>=<value>
k label no <node> <key>=<value>

# 라벨 변경시 --overwrite 옵션 추가
k label <object> <pod> <key>=<value> --overwrite
```

### 주석 (annotate)

```
k annotate <object> <pod> <key>=<value>
k annotate pod <pod> <key>="<value with whitespace>"
```

### 배포 관리

#### 삭제

```
k delete po --all
k delete all --all # 시크릿 등 특정 리소스는 지우지 않음
```

#### replicationcontroller

```
# generator=run/v1 deprecated 디플로이먼트 대신 레플리케이션 컨트롤러를 생성하도록 하는 역할
k run kubia --image=luksa/kubia --port=8080 --generator=run/v1
# 대안 : k apply -f kubia-rc.yml

k scale rc kubia --replicas=3

k delete rc kubia
```

#### deployment

```
k rollout restart deployment/<name>
```

#### 외부 노출

```
# 레플리케이션 컨트롤러를 서비스로 노출하고 EXTERNAL-IP를 받아옴
# minikube는 kubectl 로드 밸런서 서비스를 지원하지 않는데 minikube 명령어를 사용하면 서비스에 접근할 수 있는 IP를 제공한다.
k expose rc kubia --type=LoadBalancer --name kubia-http
minikube service kubia-http

k port-forward (pod) (local port):(pod port)
```

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

#### exec

```
k exec pod/<pod-name> -c <container-name> -it -- /bin/bash
k exec -it <pod-name> <command>
```

#### logs

```
k logs (pod)
k logs (pod) -c (container)
```

## 자동 완성

* kubectl 자동완성 및 alias : <https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>

```bash
$ echo 'source <(kubectl completion zsh)' >> ~/.zshrc
$ echo 'alias k=kubectl' >> ~/.zshrc
$ echo 'complete -F __start_kubectl k' >> ~/.zshrc
```

* 현재 컨텍스트와 네임스페이스를 표시 : <https://github.com/superbrothers/zsh-kubectl-prompt>
