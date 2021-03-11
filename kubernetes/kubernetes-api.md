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

### Pod 내에서 API 실행

```bash
# 권한 얻기 (production에서 쓰지 말 것)
k create clusterrolebinding permissive-binding --clusterrole=cluster-admin --group=system:serviceaccounts

# in container
curl https://kubernetes --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
```

### 앰배서더 컨테이너 패턴

* <https://github.com/luksa/kubernetes-in-action/tree/master/Chapter08/kubectl-proxy>
* <https://github.com/luksa/kubernetes-in-action/blob/master/Chapter08/curl-with-ambassador.yaml>
