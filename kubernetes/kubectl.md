# Kubectl

## Reference

* <https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/>

## 약어

```
k kubectl
rc replicationcontroller
po pods
svc services
pvc persistentvolumeclaims
```

## 명령어

### config & 정보 출력

```
# 정보 출력
k config current-context
k config get-contexts
k cluster-info
k describe po [<pod-name>]
k describe nodes [<context-name>]

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

k get po -o wide

# 기본 출력을 위한 Get 커맨드
k get pods --all-namespaces             # 모든 네임스페이스 내 모든 파드의 목록 조회
k get deployment my-dep                 # 특정 디플로이먼트의 목록 조회
k get pods                              # 네임스페이스 내 모든 파드의 목록 조회
k get pod my-pod -o yaml                # 파드의 YAML 조회
k get pod my-pod -o yaml --export       # 클러스터 명세 없이 파드의 YAML 조회
```

### create
```
k create namespace <namespace>
```

### delete

```
k delete namespaces <namespace>
```


### 배포 관리

#### replicationcontroller

```
# generator=run/v1 deprecated
k run kubia --image=luksa/kubia --port=8080 --generator=run/v1

# 레플리케이션 컨트롤러를 서비스로 노출하고 EXTERNAL-IP를 받아옴
# minikube는 kubectl 로드 밸런서 서비스를 지원하지 않는데 minikube 명령어를 사용하면 서비스에 접근할 수 있는 IP를 제공한다.
k expose rc kubia --type=LoadBalancer --name kubia-http
minikube service kubia-http

k scale rc kubia --replicas=3
```

#### deployment

```
k rollout restart deployment/<name>
```

### Pod, Container 관리

* Pod 이름을 호스트 이름으로 사용

#### exec

```
k exec pod/<pod-name> -c <container-name> -it -- /bin/bash
```

#### logs

```
k logs (pod)
k logs (pod) -c (container)
```

### etc

```
k port-forward (pod) (local port):(pod port)
```

## 자동 완성

* kubectl 자동완성 및 alias : <https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>

```bash
$ echo 'source <(kubectl completion zsh)' >> ~/.zshrc
$ echo 'alias k=kubectl' >> ~/.zshrc
$ echo 'complete -F __start_kubectl k' >> ~/.zshrc
```
