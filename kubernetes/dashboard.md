# dashboard

## install

<https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/#%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C-ui-%EB%B0%B0%ED%8F%AC>

```bash
# 배포
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta6/aio/deploy/recommended.yaml

# status
$ kubectl get pod --namespace=kubernetes-dashboard -l k8s-app=kubernetes-dashboard
NAME                                  READY   STATUS    RESTARTS   AGE
kubernetes-dashboard-b65488c4-j7r22   1/1     Running   0          3m20s

# proxy : http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

## create account

<https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md>

```bash
$ kubectl apply -f dashboard-adminuser.yaml 
serviceaccount/admin-user created

$ kubectl apply -f dashboard-adminuser-role.yaml 
clusterrolebinding.rbac.authorization.k8s.io/admin-user created

$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

## minikube

<https://kubernetes.io/ko/docs/setup/learning-environment/minikube/#%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C>

```bash
$ minikube dashboard
```