# Kubectl

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

# 기본 출력을 위한 Get 커맨드
k get pods --all-namespaces             # 모든 네임스페이스 내 모든 파드의 목록 조회
k get pods -o wide                      # 네임스페이스 내 모든 파드의 상세 목록 조회
k get deployment my-dep                 # 특정 디플로이먼트의 목록 조회
k get pods                              # 네임스페이스 내 모든 파드의 목록 조회
k get pod my-pod -o yaml                # 파드의 YAML 조회
k get pod my-pod -o yaml --export       # 클러스터 명세 없이 파드의 YAML 조회
```

### logs

```
k logs (pod)
k logs (pod) -c (container)
```

### etc

```
k port-forward (pod) (local port):(pod port)
```

### deployment

```
k rollout restart deployment/<name>
k get pods

k exec pod/<pod-name> -c <container-name> -it -- /bin/bash
```

## minikube

```
minikube start
```

# 레플리케이션 컨트롤러
k run kubia --image=luksa/kubia --port=8080 --generator=run/v1

# 레플리케이션 컨트롤러를 서비스로 노출하고 EXTERNAL-IP를 받아옴
k expose rc kubia --type=LoadBalancer --name kubia-http

디플로이먼트

## 자동 완성

* kubectl 자동완성 및 alias : <https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>

```bash
$ echo 'source <(kubectl completion zsh)' >> ~/.zshrc
$ echo 'alias k=kubectl' >> ~/.zshrc
$ echo 'complete -F __start_kubectl k' >> ~/.zshrc
```
