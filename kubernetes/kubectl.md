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
k label <resource> <name> <key>=<value>
k label po <pod> <key>=<value>
k label no <node> <key>=<value>

# 라벨 변경시 --overwrite 옵션 추가
k label <resource> <name> <key>=<value> --overwrite
``` 

### 주석 (annotate)

```
k annotate <resource> <name> <key>=<value>
k annotate pod <pod> <key>="<value with whitespace>"
```

### delete

```
k delete po --all
k delete all --all # 현재 네임스페이스 한정 / 시크릿 등 특정 리소스는 지우지 않음
k delete ns <namespace>
```

### replicationcontroller

```
# generator=run/v1 deprecated 디플로이먼트 대신 레플리케이션 컨트롤러를 생성하도록 하는 역할
k run kubia --image=luksa/kubia --port=8080 --generator=run/v1
# 대안 : k apply -f kubia-rc.yml

k scale rc kubia --replicas=3

k delete rc kubia
```

### deployment

```
k rollout restart deployment/<name>
```

### 외부 노출

```
# 레플리케이션 컨트롤러를 서비스로 노출하고 EXTERNAL-IP를 받아옴
# minikube는 kubectl 로드 밸런서 서비스를 지원하지 않는데 minikube 명령어를 사용하면 서비스에 접근할 수 있는 IP를 제공한다.
k expose rc kubia --type=LoadBalancer --name kubia-http
minikube service kubia-http

k port-forward (pod) (local port):(pod port)
```

```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=jenkins" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 8080:8080
```

### exec

```
k exec <pod-name> -- <command>
k exec <pod-name> -it -- bash
k exec <pod-name> -c <container-name> -it -- /bin/bash
k exec <pod-name> -- ls -al
```

### logs

```
k logs (pod)
k logs (pod) -c (container)
k logs (pod) --previous
```

### configmap

```
```

### secret

```
# generic
k create secret generic <secret-name> --from-file=<file>
k create secret generic fortune-https --from-file=https.key --from-file=https.cert --from-file=foo

# tls
k create secret tls <secret-name> --cert=<file> --key=<file>
k create secret tls fortune-tls --cert=https.cert --key=https.key
```

## 자동 완성

* kubectl 자동완성 및 alias : <https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>
* <https://kubernetes.io/ko/docs/tasks/tools/install-kubectl/#%EC%85%B8-%EC%9E%90%EB%8F%99-%EC%99%84%EC%84%B1-%ED%99%9C%EC%84%B1%ED%99%94>

```bash
$ echo 'source <(kubectl completion zsh)' >> ~/.zshrc
$ echo 'alias k=kubectl' >> ~/.zshrc
$ echo 'complete -F __start_kubectl k' >> ~/.zshrc
```

* 현재 컨텍스트와 네임스페이스를 표시 : <https://github.com/superbrothers/zsh-kubectl-prompt>
* <https://github.com/ahmetb/kubectx>
