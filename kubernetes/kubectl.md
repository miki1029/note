# Kubectl

## Reference

* <https://kubernetes.io/ko/docs/reference/>
* <https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/>
* <https://kubernetes.io/docs/reference/kubectl/overview/#resource-types>

## 약어

```
k api-resources

k kubectl
po pods
rc replicationcontrollers
rs replicasets
ds daemonsets
deploy deployments
svc services
pvc persistentvolumeclaims
pv persistentvolumes
sc storageclasses
sts statefulesets
ing ingresses
no nodes node
crd customresourcedefinitions

-n --namespace
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

# 이벤트
k get events --sort-by=.metadata.creationTimestamp

# 커스텀 리소스
k get crd

#
k get componentstatuses
```

* advanced

```
k get deploy <deployment> -o=jsonpath='{.spec.template.spec.nodeSelector}'

# 특정 노드의 pod 조회
k get po --all-namespaces -o wide --field-selector spec.nodeName=<nodeName>

# 모든 노드의 IP
k get no -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'
k get no -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'
```

### set

```
k set image deploy <deployment-name> <container-name>=<image>
```

### top

```
k top po
k top po --containers
k top no

# node별 조회(CPU 정렬)
kubectl get po --all-namespaces -o wide | grep <nodeName> | awk '{print $1" "$2}' | xargs -n2 kubectl top po --no-headers -n | sort --key 2 --numeric --reverse

# node별 조회(메모리 정렬)
kubectl get po --all-namespaces -o wide | grep <nodeName> | awk '{print $1" "$2}' | xargs -n2 kubectl top po --no-headers -n | sort --key 3 --numeric --reverse
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

### annotate

```
k annotate <resource> <name> <key>=<value>
k annotate pod <pod> <key>="<value with whitespace>"
```

### delete

* <https://stackoverflow.com/questions/48934491/kubernetes-how-to-delete-pods-based-on-age-creation-time>
* <https://stackoverflow.com/questions/53539576/kubectl-list-delete-all-completed-jobs>

```
k delete po --all
k delete all --all # 현재 네임스페이스 한정 / 시크릿 등 특정 리소스는 지우지 않음
k delete ns <namespace>
k delete rc <rc> --cascade=false # pod은 지우지 않음

k get jobs -o go-template --template '{{range .items}}{{.metadata.name}} {{.metadata.creationTimestamp}}{{"\n"}}{{end}}' | awk '$2 <= "2020-11-01T09:00:00,000000000Z" { print $1 }' | xargs k delete jobs
```

### drain

```
kubectl drain <node name>
```

### scale, rollout

```
k scale rc kubia --replicas=3
k scale rs kubia --replicas=3
k rollout status deployment <name>
k rollout restart deployment <name>
k rollout pause deployment <name>
k rollout resume deployment <name>
k rollout history deployment <name>
k rollout history deployment <name> --revision 3
k rollout undo deployments <name>
k rollout undo deployments <name> --to-revision 3
```

### expose, port-forward (외부 노출)

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
k create configmap <configmap-name> --from-literal=<key>=<value>
k create configmap <configmap-name> --from-file=<filename> # 디렉터리를 지정하면 디렉터리 내의 모든 파일을 등록
k create configmap <configmap-name> --from-file=<key>=<filename>
```

### secret

```
# generic
k create secret generic <secret-name> --from-file=<file>
k create secret generic fortune-https --from-file=https.key --from-file=https.cert --from-file=foo

# tls
k create secret tls <secret-name> --cert=<file> --key=<file>
k create secret tls fortune-tls --cert=https.cert --key=https.key

# openssl example
openssl genrsa -out tls.key 2048
openssl req -new -x509 -key https.key -out https.cert -days 360 -subj

# certificate example
# CertificateSigningRequest resource
# 서명된 인증서는 CSR의 status.certificate 필드에서 추출
k certificate approve <name of the CSR>

# docker-registry : .dockercfg 파일을 생성 (spec.imagePullSecrets[].name)
k create secret docker-registry <secret-name> --docker-username=<myusername> --docker-password=<mypassword> --docker-email=<myemail>
```

## 자동 완성

* kubectl 자동완성 및 alias : <https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/#%EC%85%B8-%EC%9E%90%EB%8F%99-%EC%99%84%EC%84%B1-%ED%99%9C%EC%84%B1%ED%99%94>

```bash
$ vim ~/.zshrc

# kubectl
source <(kubectl completion zsh)
alias k=kubectl
complete -F __start_kubectl k
```

* ktx, kns

```bash
$ vim ~/.zshrc
# ktx, kns
alias ktx='kubectl config use-context'
alias kns='kubectl config set-context $(kubectl config current-context) --namespace'
```

* 현재 컨텍스트와 네임스페이스를 표시 : <https://github.com/superbrothers/zsh-kubectl-prompt>

```bash
$ brew tap superbrothers/zsh-kubectl-prompt
$ brew install zsh-kubectl-prompt
$ vim ~/.zshrc

# zsh-kubectl-prompt
autoload -U colors; colors
source /usr/local/etc/zsh-kubectl-prompt/kubectl.zsh
# source /opt/homebrew/etc/zsh-kubectl-prompt/kubectl.zsh
RPROMPT='%{$fg[blue]%}($ZSH_KUBECTL_PROMPT)%{$reset_color%}'
```

* <https://github.com/ahmetb/kubectx>
