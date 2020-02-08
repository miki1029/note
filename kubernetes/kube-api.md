# Kubernetes API

## Reference

* <https://kubernetes.io/ko/docs/concepts/overview/kubernetes-api/>
* <https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/>

## 명령어

### 정보 출력

```bash
# API 정보
k api-resources
k explain po
k explain po.spec

k get po {pod} -o yaml
k get po {pod} -o json
```

### 


```bash
k create -f {file.yaml}
```

